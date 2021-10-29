# TRIM Manual

## Ativando e usando o TRIM em discos SSD no Ubuntu Linux

## Para ativar e usar o TRIM em discos SSD no Ubuntu Linux, faça o seguinte:

Passo 1. Se não estiver aberto, execute um terminal usando o Dash ou pressionando as teclas CTRL+ALT+T;

Passo 2. Confirme se você tem um SSD como o comando abaixo. Se o resultado for 0 você tem um SSD, mas se for 1 que é um HDD:

`cat /sys/block/sda/queue/rotational`

Passo 3. Mesmo se você tiver um SSD, nem todos eles suportam o TRIM. Para saber se o seu suporta executado o comando a seguir:

`sudo hdparm -I /dev/sda | grep "TRIM supported"`

Passo 4. Se o retorno for igual a mensagem abaixo, então você pode seguir adiante. Se não houver nenhuma saída, isso significa que seu SSD não suporta TRIM.

`Data Set Management TRIM supported`

Passo 5. Agora execute o comando, conforme abaixo:

`sudo fstrim -v /`

Passo 6. Você deve ver uma saída, parecida com isso:

`/: 69470965760 bytes were trimmed`

Passo 7. Se tudo correu bem, é hora de programar o cron para executar o fstrim uma vez por dia, para isso, crie um arquivo com esse comando:

`sudo nano /etc/cron.daily/trim`

Passo 8. Copie e cole as linhas abaixo no arquivo criado e salve-o. Em seguida, feche o gedit:

`#!/bin/sh
LOG=/var/log/trim.log
echo "*** $(date -R) ***" >> $LOG
fstrim -v / >> $LOG
fstrim -v /home >> $LOG`

Passo 9. Agora torne o script executável com o comando abaixo:

`sudo chmod +x /etc/cron.daily/trim`

Pronto. Agora seu sistema está com o TRIM habilitado.

Verificando se o Trim esta ativado

`systemctl status fstrim.time`
