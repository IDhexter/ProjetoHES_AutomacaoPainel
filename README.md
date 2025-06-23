# Projeto "Automação Simplificada" Hospital Evangélico

Inicialmente foi feita uma automação via "AutoHotKey", porém há dois tipos de paineis;
Um onde é aberto o Soul MV (Que necessita de automação) localizado no Pronto Atendimento e outro é exibido apenas uma Dashboard (Que não necessita de automação) que é exibida nos postos,
este segundo utilizamos uma tatica simples via agendador de tarefas, que vou chamar de "Simplificado" é este que iremos utilizar aqui, este passo a passo é um guia para quem estiver atuando na unidade.

1. Necessário fazer o 'auto-login' via registro\.BAT ou utilizando [Autologon v3.10](https://learn.microsoft.com/pt-br/sysinternals/downloads/autologon) nas estações.
Caso prefira criar a .bat, coloque o seguinte codigo e execute como administrador para inserir no registro, se atente a substituir o dominio/login/senha.

       reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v AutoAdminLogon /t REG_SZ /d 1 /f
       reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultDomainName /t REG_SZ /d SEU_DOMINIO_SE_HOUVER /f
       reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultUserName /t REG_SZ /d SEU_LOGIN /f
       reg add "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon" /v DefaultPassword /t REG_SZ /d SUA_SENHA /f

2. Crie um atalho onde ele ira abrir o navegador pelo seu caminho raiz; abrindo o site (Dashboard) desejado, e adicione o modo kioske ao final, salve como "NomeDoPainel.ink" e o movimente para a unidade C:\ ou onde preferir

"C:\Caminho do programa" https://site_do_dashboard --kiosk (Comando modo kiosk, para abrir em tela cheia)

       "C:\Program Files\Google\Chrome\Application\chrome.exe" http://2311prd.cloudmv.com.br:80/Painel_PRD/ACCOUNT/LOGIN_NEW.ASPX?chave=Lz60TZqzQj4P7ExDB1t4dkSLB3xGGyOfs0c9XJvPapiU2K5uKS7NSjMpfsj1W5My5Je2PHZHXtXmkqz1BfYuww%3d%3d --kiosk


3. Crie uma .bat que ira chamar esse atalho sempre que for executado, e a movimente para a unidade C:\ ou onde preferir

Coloque o caminho com o nome correto do seu atalho aqui usamos a unidade C\ e o nome PainelMV.ink

       @echo off
       start "" "C:\PainelMV.lnk"
       exit

4. No gerenciador de Tarefas do Windows (taskschd.msc), crie um agendamento para sempre executar esse scrip apos o logon do usuario.

![Logon](https://i.ibb.co/pjypB6Hc/Apos-Logon.jpg)

5. Não é estritamente necessário, mas após 24h o painel tende a perder a chave de acesso a dashboard, dando falha no painel; para evitar isso basta criar um reboot no agendador de tarefas (taskschd.msc), eu fiz um reboot a cada 8h.

![Reboot](https://i.ibb.co/Q37wqJdS/Reboot.jpg)

