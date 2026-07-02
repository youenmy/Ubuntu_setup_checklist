# Ubuntu_setup_checklist
1. Обновляем
```apt update && apt upgrade -y && apt autoremove -y && reboot``` #обновить список пакетов из репы, обновить, удалить зависимости свободные, небут на случай если обноавлялось ядро

2. Добавляем стой паблик ключ 
```mkdir -p /root/.ssh && chmod 700 /root/.ssh``` #создаёт папку (-p = не ругаться, если уже есть)&&доступ к папке только владельцу (иначе SSH ключ проигнорирует)
```echo "ssh-ed25519 тутключ" >> /root/.ssh/authorized_keys``` #дописывает публичный ключ (>> = в конец, не стирая существующие)
```chmod 600 /root/.ssh/authorized_keys``` #права на файл только для владельца
```chown -R root:root /root/.ssh #владелец root```

3. Отключить вход по паролю+включить вход по ключу+перезапуск SSH
```printf 'PasswordAuthentication no\nPubkeyAuthentication yes\nPermitRootLogin prohibit-password\n' | sudo tee /etc/ssh/sshd_config.d/99-hardening.conf > /dev/null && sudo sshd -t && sudo systemctl restart ssh```







