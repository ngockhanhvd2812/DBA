````
#!/usr/bin/env bash
set -euo pipefail

export LANG=en_US.UTF-8
export LC_MESSAGES=C

TIMEOUT_SEC="${TIMEOUT_SEC:-3}"
OUTFILE="portcheck_$(date +%Y%m%d_%H%M%S).csv"

need() { command -v "$1" >/dev/null 2>&1 || { echo "Thiếu $1. Cài: sudo dnf install -y $2"; exit 1; }; }
need telnet telnet
need timeout coreutils

to_vi() {
  case "$1" in
    OPEN) echo "MỞ" ;;
    CLOSED) echo "ĐÓNG" ;;
    "TIMEOUT/FILTERED") echo "TIMEOUT/FILTERED" ;;
    UNREACHABLE) echo "KHÔNG ĐẠT" ;;
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
print_hdr() { printf "%-8s %-15s %-5s %-18s %s\n" "PROTO" "HOST" "PORT" "TRẠNG THÁI" "GHI CHÚ"; printf "%0.s-" {1..70}; echo; }
print_line(){ printf "%-8s %-15s %-5s %-18s %s\n" "$1" "$2" "$3" "$4" "$5"; }

# =========================
# Danh sách mục tiêu (CHỈ TCP)
# =========================
declare -a TARGETS=(
  "10.169.20.229|TCP|514,8413|SIEM"
  "10.169.20.230|TCP|514,8413|SIEM"
  "10.159.25.10|TCP|10050,10051|Zabbix"
  "10.159.136.10|TCP|10050,10051|Zabbix"
  "10.169.20.226|TCP|13000,14000|KAS"
)
for i in $(seq 21 25); do TARGETS+=("10.165.73.$i|TCP|111,2049,20048|NetBackup"); done
for i in $(seq 11 13); do TARGETS+=("10.168.12.$i|TCP|111,2049,20048|NetBackup"); done

# Thu thập danh sách host duy nhất
declare -A HOST_SET=()
for item in "${TARGETS[@]}"; do
  IFS="|" read -r HOST _ PORTS _ <<< "$item"
  HOST_SET["$HOST"]=1
done

# Bổ sung port Mongo/Oracle cho MỌI host đích
for HOST in "${!HOST_SET[@]}"; do
  TARGETS+=("$HOST|TCP|27010|Mongo Sharded Cluster")
  TARGETS+=("$HOST|TCP|27011,27012,27013|Mongo (bổ trợ)")
  TARGETS+=("$HOST|TCP|1521|Oracle DB")
done

# =========================
# Chạy kiểm tra (local -> đích)
# =========================
echo "\"timestamp\",\"proto\",\"host\",\"port\",\"status\",\"note\"" > "$OUTFILE"
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
echo "Đã lưu kết quả: $OUTFILE"
echo "Chế độ: chỉ TCP, 1 chiều (máy này -> IP đích) bằng telnet."


```