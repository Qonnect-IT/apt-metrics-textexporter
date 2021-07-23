#!/bin/bash -e
# /usr/share/apt-metrics

APT_CHECK=$(/usr/lib/update-notifier/apt-check 2>&1 || echo "0;0")

UPDATES=$(echo "$APT_CHECK" | cut -d ';' -f 1)
SECURITY=$(echo "$APT_CHECK" | cut -d ';' -f 2)
REBOOT=$([ -f /var/run/reboot-required ] && echo 1 || echo 0)

UPDATESTOTAL="$(/usr/bin/apt-get --just-print dist-upgrade \
  | /usr/bin/awk -F'[()]' \
      '/^Inst/ { sub("^[^ ]+ ", "", $2); gsub(" ","",$2);
                 sub("\\[", " ", $2); sub("\\]", "", $2); print $2 }' \
  | /usr/bin/sort \
  | /usr/bin/uniq -c \
  | awk '{ gsub(/\\\\/, "\\\\", $2); gsub(/"/, "\\\"", $2);
           gsub(/\[/, "", $3); gsub(/\]/, "", $3);
           print "apt_upgrades_pending{origin=\"" $2 "\",arch=\"" $NF "\"} " $1}'
)"
AUTOREMOVE="$(/usr/bin/apt-get --just-print autoremove \
  | /usr/bin/awk '/^Remv/{a++}END{printf "apt_autoremove_pending %d", a}'
)"

echo '# HELP apt_upgrades_pending_total Apt package pending updates by origin.'
echo '# TYPE apt_upgrades_pending_total gauge'
if [[ -n "${UPDATESTOTAL}" ]] ; then
  echo "${UPDATESTOTAL}"
else
  echo 'apt_upgrades_pending_total{origin="",arch=""} 0'
fi

echo '# HELP apt_autoremove_pending Apt package pending autoremove.'
echo '# TYPE apt_autoremove_pending gauge'
echo "${AUTOREMOVE}"

echo "# HELP apt_upgrades_pending Apt package pending updates."
echo "# TYPE apt_upgrades_pending gauge"
echo "apt_upgrades_pending ${UPDATES}"

echo "# HELP apt_security_upgrades_pending Apt package pending security updates by origin."
echo "# TYPE apt_security_upgrades_pending gauge"
echo "apt_security_upgrades_pending ${SECURITY}"

echo "# HELP node_reboot_required Node reboot is required for software updates."
echo "# TYPE node_reboot_required gauge"
echo "node_reboot_required ${REBOOT}"
