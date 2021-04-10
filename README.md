# Shrink_Online_VPS_FS

Если это удаленный сервер без консоли, вы можете сжать файловую систему с помощью initramfs во время перезагрузки. 
Вам нужно добавить resize2fs в initramfs и запустить его перед установкой root.

Пример Debian /Ubuntu:

/etc /initramfs-tools /hooks /resizefs (исполняемый файл):

#!/bin/sh

set -e

PREREQS=""

prereqs() { echo "$PREREQS"; }

case $1 in
    prereqs)
        prereqs
        exit 0
    ;;
esac

. /usr/share/initramfs-tools/hook-functions

copy_exec /sbin/e2fsck
copy_exec /sbin/resize2fs

exit 0
/etc /initramfs-tools /scripts /local-premount /resizefs (исполняемый файл)

#!/bin/sh

set -e

PREREQS=""

prereqs() { echo "$PREREQS"; }

case "$1" in
    prereqs)
        prereqs
        exit 0
    ;;
esac

/sbin/e2fsck -yf /dev/sda1
/sbin/resize2fs /dev/sda1 5G
/sbin/e2fsck -yf /dev/sda1
Теперь запустите update-initramfs, перезагрузите, проверьте размер, снова удалите эти скрипты и update-intiramfs.

Для dracut (Fedora, CentOS) используйте ту же логику (/usr/lib/dracut/modules.d).

Это очень полезно для шифрования или перемещения rootfs, без свободного места для нового раздела.
