Este guia ensina como montar seu Google Drive como uma pasta local no Windows (C:), com cache para performance e iniciando automaticamente com o sistema.


### O Resultado
Você terá uma pasta na nuvem que se comporta como um HD físico, usando o **Rclone** e operando dentro das políticas da Google. 

> **Nota:** A própria Google disponibiliza as chaves de acesso via [Google Drive API](https://developers.google.com/drive/api/guides/about-sdk) para ferramentas como o Rclone.

---

## Curiosidade Técnica: Por que não uso o app oficial?

### Microsoft (OneDrive)
A Microsoft integra o **OneDrive** diretamente no *Windows Shell* (o núcleo da interface gráfica), permitindo que ele funcione nativamente sem configurações extras.

### Google (Drive for Desktop)
A Google disponibiliza o *Google Drive for Desktop*, que também permite acesso local. Porém:
1. Ele consome muita memória RAM.
2. Não permite controle granular sobre o download de dados.
3. Caso queira usar a memória de forma avançada, o kernel não alcança essa nuvem devido à falta de integração com o Windows.

### 🏆 A Vantagem deste Método (Rclone + WinFsp)
Diferente do app oficial, este método usa o **WinFsp** para criar um sistema de arquivos virtual que obedece aos *seus* parâmetros. É isso que nos permite definir regras agressivas de cache e manter o sistema leve, algo impossível no software padrão da Google.

---

## Downloads necessários

Você precisará de duas ferramentas open source:

* **Rclone:** [Baixe aqui](https://rclone.org/downloads/)
* **WinFsp:** [Baixe aqui](https://winfsp.dev/rel/)

*Extraia o Rclone para uma pasta segura (ex: `C:\rclone\`) e instale o WinFsp.*


## Configurando o Acesso

Abra o terminal (CMD) na pasta do Rclone e digite:

```bash

rclone config
Digite n para "New Remote"
Nomeie como gdrive ou o nome que preferir
Procure o número correspondente ao Google Drive na lista
Siga os passos de autenticação no navegador que abrirá

```

## Script de inicialização

@echo off
:: Coloque aqui o caminho onde está seu rclone.exe
:: Exemplo: C:\rclone ou C:\Users\%USERNAME%\Documents\rclone
cd /d "C:\caminho\pasta\do\Rclone"

:: Cria a pasta "Nuvem" no seu usuário se ela não existir
if not exist "%USERPROFILE%\nuvem" mkdir "%USERPROFILE%\nuvem"

:: --- COMANDO DE MONTAGEM ---
:: gdrive: nome que você deu no 'rclone config'
:: %USERPROFILE%\Nuvem: onde vai aparecer a pasta (ex: C:\Users\voce\nuvem)

start rclone.exe mount gdrive: "%USERPROFILE%\nuvem" ^
--vfs-cache-mode full ^
--vfs-cache-max-size 188G ^
--vfs-read-ahead 500M ^


## Script de inicialização

Para não precisar clicar no .bat toda vez que ligar o PC:

Pressione Win + R no teclado.

Digite shell:startup e dê Enter.

Mova o seu arquivo montarDrive.bat para dentro dessa pasta que abriu.


