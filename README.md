# Ubuntu_setup_checklist
1. Обновляем
apt update && apt upgrade -y && apt autoremove -y && reboot #обновить список пакетов из репы, обновить, удалить зависимости свободные, ребут на случай если обноавлялось ядро

2. Добавляем стой паблик ключ 
mkdir -p /root/.ssh && chmod 700 /root/.ssh #создаёт папку (-p = не ругаться, если уже есть)&&доступ к папке только владельцу (иначе SSH ключ проигнорирует)
echo "ssh-ed25519 тутключ" >> /root/.ssh/authorized_keys``` #дописывает публичный ключ (>> = в конец, не стирая существующие)
chmod 600 /root/.ssh/authorized_keys #права на файл только для владельца
chown -R root:root /root/.ssh #владелец root

3. Отключить вход по паролю+включить вход по ключу+перезапуск SSH
printf 'PasswordAuthentication no\nPubkeyAuthentication yes\nPermitRootLogin prohibit-password\n' | sudo tee /etc/ssh/sshd_config.d/99-hardening.conf > /dev/null && sudo sshd -t && sudo systemctl restart ssh

4.Фаервол
ufw enable && ufw allow 22/tcp #включить фаервол и разрешить 22 порт
echo 'net.ipv4.icmp_echo_ignore_all=1' | tee /etc/sysctl.d/99-noping.conf > /dev/null && sysctl --system #отключить icmp

htop и btop - мониторы ресурсов, второй более красивый

Создать и Включить Swop:
fallocate -l 1G /swapfile && \ #Создаём файл на 1 ГБ
chmod 600 /swapfile && \ #Закрываем права — только root
mkswap /swapfile && \ #Размечаем файл как swap
swapon /swapfile && \ #Включаем
echo '/swapfile none swap sw 0 0' >> /etc/fstab && \ #Прописываем в fstab, чтобы поднимался после ребута
echo 'vm.swappiness = 10' >> /etc/sysctl.conf && \ #Приглушаем агрессивность свопа
sysctl vm.swappiness=10 
free -h














