O Trim foi introduzido logo após o lançamento dos SSDs. Como a operação de baixo nível de SSDs difere significativamente de discos rígidos, a maneira típica em que os sistemas operacionais lidam com operações como exclusões e formatos resultou na degradação progressiva imprevista do desempenho das operações de gravação em SSDs. O corte permite que o SSD lide com a coleta de lixo de maneira mais eficiente , o que, de outra forma, tornaria as operações de gravação futuras nos blocos envolvidos.

O comando TRIM permite que um sistema operacional notifique o SSD de páginas que não contêm mais dados válidos. Para uma operação de exclusão de arquivo , o sistema operacional marcará os setores do arquivo como livres para novos dados e enviará um comando TRIM ao SSD. Após o corte, o SSD não preservará nenhum conteúdo do bloco ao gravar novos dados em uma página da memória flash, resultando em menos amplificação de gravação (menos gravações), maior capacidade de gravação (sem necessidade de uma sequência de leitura-apagamento-modificação), aumentando assim a vida útil da unidade.



# TRIM Manual

## Ativando e usando o TRIM em discos SSD no Ubuntu Linux

## Para ativar e usar o TRIM em discos SSD no Linux, faça o seguinte:

Passo 1. *Se não estiver aberto, execute um terminal usando o Dash ou pressionando as teclas CTRL+ALT+T;*

Passo 2. *Confirme se você tem um SSD como o comando abaixo. Se o resultado for 0 você tem um SSD, mas se for 1 que é um HDD:*

`cat /sys/block/sda/queue/rotational`

Passo 3. *Mesmo se você tiver um SSD, nem todos eles suportam o TRIM. Para saber se o seu suporta executado o comando a seguir:*

`sudo hdparm -I /dev/sda | grep "TRIM supported"`

Passo 4. *Se o retorno for igual a mensagem abaixo, então você pode seguir adiante. Se não houver nenhuma saída, isso significa que seu SSD não suporta TRIM.*

*`Data Set Management TRIM supported`*

Passo 5. *Agora execute o comando, conforme abaixo:*

`sudo fstrim -v /`

Passo 6. *Você deve ver uma saída, parecida com isso:*

`/: 69470965760 bytes were trimmed`

Passo 7. *Se tudo correu bem, é hora de programar o cron para executar o fstrim uma vez por dia, para isso, crie um arquivo com esse comando:*

`sudo nano /etc/cron.daily/trim`

Passo 8. *Copie e cole as linhas abaixo no arquivo criado e salve-o. Em seguida, feche o nano:*

`#!/bin/sh
LOG=/var/log/trim.log
echo "*** $(date -R) ***" >> $LOG
fstrim -v / >> $LOG
fstrim -v /home >> $LOG`

Passo 9. *Agora torne o script executável com o comando abaixo:*

`sudo chmod +x /etc/cron.daily/trim`

Pronto. *Agora seu sistema está com o TRIM habilitado.*

*Verificando se o Trim esta ativado*

`systemctl status fstrim.time`
