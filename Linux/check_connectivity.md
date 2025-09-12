````
#!/usr/bin/env bash
set -euo pipefail

export LANG=en_US.UTF-8
export LC_MESSAGES=C

TIMEOUT_SEC="${TIMEOUT_SEC:-3}"
OUTFILE="portcheck_$(date +%Y%m%d_%H%M%S).csv"

# --- Hàm hỗ trợ ---
need() { command -v "$1" >/dev/null 2>&1 || { echo "Lỗi: Thiếu lệnh '$1'. Cài đặt: sudo dnf install -y $2"; exit 1; }; }
need telnet telnet
need timeout coreutils

to_vi() {
  case "$1" in
    OPEN) echo "MỞ" ;;
    CLOSED) echo "ĐÓNG (Connection refused)" ;;
    "TIMEOUT/FILTERED") echo "TIMEOUT / BỊ LỌC" ;;
    UNREACHABLE) echo "KHÔNG THỂ KẾT NỐI (Lỗi DNS/mạng)" ;;
    *) echo "KHÔNG XÁC ĐỊNH" ;;
  esac
}

telnet_check() {
  local host="$1" port="$2" to="$3"
  local out rc
  set +e
  out=$( (sleep 0.2; echo quit) | timeout "${to}s" telnet "$host" "$port" 2>&1 )
  rc=$?
  set -e

  if [[ $rc -eq 124 ]]; then
    echo "TIMEOUT/FILTERED"
  elif grep -qiE "Connected to|Escape character" <<<"$out"; then
    echo "OPEN"
  elif grep -qi "Connection refused" <<<"$out"; then
    echo "CLOSED"
  elif grep -qiE "No route to host|Network is unreachable|Name or service not known|Unknown host|Temporary failure in name resolution|bad address|hostname nor servname provided" <<<"$out"; then
    echo "UNREACHABLE"
  else
    echo "UNKNOWN"
  fi
}

log_row()   { echo "\"$1\",\"$2\",\"$3\",\"$4\",\"$5\",\"$6\"" >> "$OUTFILE"; }
print_hdr() { printf "%-8s %-15s %-5s %-28s %s\n" "GIAO THỨC" "MÁY CHỦ ĐÍCH" "CỔNG" "TRẠNG THÁI" "GHI CHÚ"; printf "%0.s-" {1..80}; echo; }
print_line(){ printf "%-8s %-15s %-5s %-28s %s\n" "$1" "$2" "$3" "$4" "$5"; }

# --- Danh sách mục tiêu cố định ---
declare -a TARGETS=(
  "10.169.20.229|TCP|514,8413|SIEM"
  "10.169.20.230|TCP|514,8413|SIEM"
  "10.159.25.10|TCP|10050,10051|Zabbix"
  "10.159.136.10|TCP|10050,10051|Zabbix"
  "10.169.20.226|TCP|13000,14000|KAS"
)
for i in $(seq 21 25); do TARGETS+=("10.165.73.$i|TCP|111,2049,20048|NetBackup"); done
for i in $(seq 11 13); do TARGETS+=("10.168.12.$i|TCP|111,2049,20048|NetBackup"); done

# --- Yêu cầu người dùng nhập IP cho các dịch vụ cụ thể ---
echo "------------------------------------------------------------------"
echo "Vui lòng nhập danh sách IP/hostname cho các dịch vụ bên dưới."
echo "Các IP/hostname có thể ngăn cách bởi dấu phẩy (,) hoặc khoảng trắng."
echo "------------------------------------------------------------------"

# Nhập IP cho MongoDB
read -p "Nhập danh sách IP máy chủ MongoDB: " mongo_ips_input
if [[ -z "$mongo_ips_input" ]]; then
  echo "Lỗi: Bạn phải nhập ít nhất một IP cho MongoDB." >&2
  exit 1
fi
# Chuyển chuỗi thành mảng, loại bỏ dấu phẩy và khoảng trắng thừa
read -ra MONGO_HOSTS <<< "${mongo_ips_input//,/ }"
for host in "${MONGO_HOSTS[@]}"; do
  # Bỏ qua các phần tử rỗng nếu người dùng nhập ", ,"
  if [[ -n "$host" ]]; then
    TARGETS+=("$host|TCP|27010,27011,27012,27013|Mongo Sharded Cluster")
  fi
done

# Nhập IP cho Oracle DB
read -p "Nhập danh sách IP máy chủ Oracle DB: " oracle_ips_input
if [[ -z "$oracle_ips_input" ]]; then
  echo "Lỗi: Bạn phải nhập ít nhất một IP cho Oracle DB." >&2
  exit 1
fi
read -ra ORACLE_HOSTS <<< "${oracle_ips_input//,/ }"
for host in "${ORACLE_HOSTS[@]}"; do
  if [[ -n "$host" ]]; then
    TARGETS+=("$host|TCP|1521|Oracle DB")
  fi
done
echo "------------------------------------------------------------------"


# --- Thực thi kiểm tra ---
echo "Bắt đầu kiểm tra kết nối TCP..."
echo "Timestamp (CSV),Protocol,Host,Port,Status (English),Note" > "$OUTFILE"
print_hdr
for item in "${TARGETS[@]}"; do
  IFS="|" read -r HOST PROTO PORTS NOTE <<< "$item"
  IFS=',' read -ra PORT_ARR <<< "$PORTS"
  for PORT in "${PORT_ARR[@]}"; do
    TS="$(date +'%F %T')"
    STATUS_EN="$(telnet_check "$HOST" "$PORT" "$TIMEOUT_SEC")"
    STATUS_VI="$(to_vi "$STATUS_EN")"
    print_line "$PROTO" "$HOST" "$PORT" "$STATUS_VI" "$NOTE"
    log_row "$TS" "$PROTO" "$HOST" "$PORT" "$STATUS_EN" "$NOTE"
  done
done

echo
echo "Kiểm tra hoàn tất."
echo "Kết quả đã được lưu vào file: $OUTFILE"

```