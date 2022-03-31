Passo 1 — Instalando o Jenkins
A versão do Jenkins incluída com os pacotes padrão do Ubuntu está, frequentemente, atrás da versão mais recente disponível do projeto. Para garantir que você tenha as últimas correções e recursos, use os pacotes mantidos pelo projeto para instalar o Jenkins.

Primeiro, adicione a chave do repositório ao sistema:

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
Depois que a chave for adicionada, o sistema irá retornar com um OK.

Em seguida, adicione o endereço do repositório de pacotes Debian ao sources.list do servidor:

sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
Depois que os dois comandos foram digitados, executaremos o update para que o apt use o novo repositório.

sudo apt update
Finalmente, instalaremos o Jenkins e suas dependências.

sudo apt install jenkins
Agora que o Jenkins e suas dependências estão funcionando, vamos iniciar o servidor Jenkins.

Passo 2 — Inicializando o Jenkins
Vamos iniciar o Jenkins usando o systemctl:

sudo systemctl start jenkins
Como o systemctl não mostra saída de status, vamos utilizar o comando status para verificar se o Jenkins iniciou com sucesso:

sudo systemctl status jenkins
Se tudo correu bem, o começo da saída de status deve mostrar que o serviço está ativo e configurado para iniciar na inicialização:

Output
● jenkins.service - LSB: Start Jenkins at boot time
   Loaded: loaded (/etc/init.d/jenkins; generated)
   Active: active (exited) since Fri 2020-06-05 21:21:46 UTC; 45s ago
     Docs: man:systemd-sysv-generator(8)
    Tasks: 0 (limit: 1137)
   CGroup: /system.slice/jenkins.service
Agora que o Jenkins está funcionando, vamos ajustar nossas regras de firewall para que possamos acessá-lo a partir de um navegador Web para completar a configuração inicial.

Passo 3 — Abrindo o firewall
Para configurar um firewall UFW, veja o tutorial Initial Server Setup with Ubuntu 20.04, Step 4- Setting up a Basic Firewall Por padrão, o Jenkins é executado na porta 8080. Abriremos essa porta usando o ufw:

sudo ufw allow 8080
Nota: Se o firewall estiver inativo, os comandos a seguir permitirão o OpenSSH e habilitarão o firewall:

sudo ufw allow OpenSSH
sudo ufw enable
Verifique o status do ufw para confirmar as novas regras:

sudo ufw status
Você vai notar que o tráfego está autorizado para a porta 8080 a partir de qualquer lugar:

Output
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
8080                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
8080 (v6)                  ALLOW       Anywhere (v6)
Com o Jenkins instalado e nosso firewall configurado, podemos completar o estágio de instalação e mergulhar na configuração do Jenkins.

Passo 4 — Configurando o Jenkins
Para configurar sua instalação, visite o Jenkins na sua porta padrão, 8080, usando o nome de domínio ou endereço IP do seu servidor: http://your_server_ip_or_domain:8080

Você deve receber a tela Unlock Jenkins, que exibe a localização da senha inicial:

Unlock Jenkins screen

Na janela do terminal, utilize o comando cat para mostrar a senha:

sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Copie a senha alfanumérica de 32 caracteres do terminal e cole-a no campo Administrator password, e então clique em Continue.

A tela seguinte apresenta a opção de instalar plug-ins sugeridos ou selecionar plug-ins específicos:

Customize Jenkins Screen

Vamos clicar na opção Install suggested plugins, que iniciará imediatamente o processo de instalação:

Jenkins Getting Started Install Plugins Screen

Quando a instalação for concluída, você será solicitado a configurar o primeiro usuário administrativo. É possível ignorar este passo e continuar como admin usando a senha inicial que usamos acima, mas vamos gastar um tempo para criar o usuário.

Nota: o servidor padrão Jenkins NÃO é criptografado, então os dados apresentados com este formulário não estão protegidos. Consulte o tutorial How to Configure Jenkins with SSL Using an Nginx Reverse Proxy on Ubuntu 20.04 para proteger as credenciais de usuário e informações sobre as compilações que são transmitidas através da interface Web.

Jenkins Create First Admin User Screen

Digite o nome e senha para seu usuário:

Jenkins Create User

Você verá uma página Instance Configuration que pedirá que você confirme o URL preferido para sua instância Jenkins. Confirme o nome do domínio para seu servidor ou o endereço IP do seu servidor:

Jenkins Instance Configuration

Após confirmar as informações apropriadas, clique em Save and Finish. Você receberá uma página de confirmação informando “Jenkins is Ready!”:

Jenkins is ready screen

Clique em Start using Jenkins para visitar o painel principal do Jenkins:

Welcome to Jenkins Screen

Neste ponto, você completou a instalação do Jenkins com sucesso.

