# Segurança de Rede

É importante cuidar da segurança do **roteador de borda**, pois ele é o último roteador entre a rede interna e uma rede não confiável. Todo o tráfego de internet de uma organização passa por um roteador de borda.

A forma como o roteador de borda é implementado varia dependendo do tamanho da organização e da complexidade do projeto de rede necessário.

- Pode ser um roteador único (SOHO)
  

- Uma abordagem em defesa em profundidade (normalmente um roteador de borda, um firewall e um roteador que se conecte a LAN protegida.)
  

- Uma abordagem em DMZ
  

O Firewall executa uma filtragem adicional, fornecendo controle de acesso adicional, rastreando o estado das conexões e atua como um dispositivo de ponto de verificação.

Outras ferramentas de segurança podem ser implementadas: sistemas de intrusões (IPSs), dispositivos de segurança Web (servidores proxy) e dispositivos de segurança de e-mail (filtragem de spam), também podem ser implementadas.

A DMZ pode ser configurada entre dois roteadores, um conectado a rede interna e outro a rede externa. O Firewall permanece entre esses dois roteadores fornecendo proteção primária para todos os dispositivos da DMZ.

### Três áreas de segurança do roteador

#### Segurança física

Colocar o roteador em uma sala trancada e segura.

#### Software de Sistema Operacional

- Equipar roteadores com a quantidade máxima de memória possível. Isso pode ajudar a proteger contra ataques DDoS, enquanto suporta a mais ampla gama de serviços de segurança.
  
- Versão atualizada do Sistema Operacional.
  
- Mantenha uma cópia segura das imagens do sistema operacional do roteador e dos arquivos de configuração.
  

#### Endurecimento do roteador

- Elimine potencial abuso de portos e serviços não utilizados.
  
  - Controle administrativo seguro
    
  - Desativar portas e interfaces não utilizadas
    
  - Desativar serviços desnecessários.
    

### Acesso administrativo Seguro

- Restringir a acessibilidade do dispositivo - Limitar portas acessíveis, restringir os comunicadores permitidos e restringir os métodos permitidos de acesso.
  
- Registro e conta para todos os acessos - Gravar tudo o que acontece no dispositivo.
  
- Autenticar o acesso - O acesso deve ser concedido somente a usuários, grupos e serviços autenticados. Limite o número de tentativas de login com falha e o tempo permitido entre logins.
  
- Autorizar ações - Restringir ações e exibições permitidas por qualquer usuário
  
- Apresentar notificação legal - Exibir um aviso legal para diferentes tipos de acesso ao dispositivo
  
- Garanta a confidencialidade dos dados - Proteger os dados armazenados localmente de serem visualizados e copiados.
  

### Segurança de acesso remoto e local

Alguns protocolos de acesso remoto enviam dados, incluindo nomes de usuário e senhas, para o roteador em texto simples. Nesse caso, um invasor pode coletar o tráfego e capturar senhas e informações das configurações do roteador.

- Criptografe todo o tráfego entre o computador e o roteador: use SSH ou HTTPS.
  
- Estabelecer uma rede de gestão dedicada. Rede com acesso rigorosamente controlado.
  
- Configurar um filtro de pacote de informações para permitir que somente os anfitriões de administração identificados e os protocolos preferidos alcancem o roteador. Exemplo é somente permitir um IP específico que inicie uma conexão com Roteadores de rede.
  
- Configurar e estabelecer uma conexão VPN à rede local antes de se conectar a uma interface de gerenciamento de roteador.
  

### Colocando senha no modo EXEC do usuário

- Line console 0 // O "0" representa a primeira interface do console.
  
- password *password*
  
- login
  

### Colocando senha no modo EXEC privilegiado

- enable secret *password*
  

Para proteger linhas VTY, entre no modo VTY usando o comando de configuração:

- line vty 0 15
  
- password *password*
  
- login
  

### Criptografar senhas

Métodos de segurança:

- Criptografar senhas de texto sem formatação
  
- Definir um tamanho mínimo e aceitável de senha
  
- Deterção de ataques de adivinhação de senha de força bruta
  
- Desativando um acesso de modo EXEC privilegiado inativo após um período especificado.
  

Os arquivos **startup-config** e **running-config** exibem a maioria das senhas em texto simples.

Para criptografar as senhas em texto simples, use o comando:

- service password-encryption
  

Esse comando aplica criptografia fraca a todas as senhas não criptografadas. Essa criptografia se aplica apenas à senhas no arquivo de configuração, não às senhas enviadas pela rede.

Use o comando abaixo para verificar se as senhas estão criptografadas

- show running-config
  

### Segurança de Senha Adicional

Para garantir que todas as senhas configuradas são um mínimo de um comprimento especificado, use o comando:

- security passwords min-length lenght
  

Esse comando deve ser utilizado no modo de configuração global.

Atores de ameaças podem usar softwares de quebra de senha para realizar ataques de força bruta, para tentar impedir isso, use o comando:

- login block-for *seconds* attempts *number* within seconds
  

Por padrão, roteadores Cisco fazem logout do modo EXEC após 10 minutos de inatividade, no entanto é possível reduzir essa configuração usando o comando:

- exec-timeout *minutes seconds*
  

### Algoritmos de senha secreta

O comando enable secret mencionado usa um hash MD5 por padrão. É recomendável criptografar as senhas utilizando senhas do tipo 8 e tipo 9, que foram introduzidas com o Cisco IOS 15.

O tipo 8 e o tipo 9 usam a criptografia SHA.

para criptografas basta digitar o seguinte comando para uma senha já especificada

- enable secret 9
  

Para inserir uma senha não criptografada use o comando:

- enable algorithm-type {md5 | scrypt | sha256 | secret } *unencrypted password*
  
  - md5 - seleciona o algoritmo de resumo de mensagens 5 (MD5) como o algoritmo de hash
    
  - scrypt - Tipo 9, seleciona scrypt como o algoritmo de hash
    
  - sha256 - Tipo 8; Seleciona a função de derivação de chave baseada em senha 2 (PBKDF2) com algoritmo hash seguro, 256 bits (SHA-256) como algoritmo de hash.
    

Exemplo de comando de alteração de senha:

- enable algoritm-type scrypt secret cisco12345
  

A criptografia do tipo 8 e 9 foram introduzidas no IOS 15 com o comando:

- username secret
  

Nesse caso a criptografia padrão será MD5, use o seguinte comando para alterar para a criptografia de tipo 9:

- username name algorithm-type
  

### Laboratório prático

Configurar acesso administrativo seguro no R2:

**1ª - De início, é necessário criptografar todas as senhas:**

R2(config)# service password-encryption

2**° - Depois é necessário definir o comprimento mínimo da senha para 10 caracteres**

R2(config)# security passwords min-length 10

**3° - Criar uma conta do usuario JR-ADMIN com uma senha secreta do cisco12345 usando o algoritmo do hashing.**

****

R2(config)# username JR-ADMIN algorithm-type scrypt secret cisco12345

**4° - Crie a conta de usuário ADMIN com uma senha secreta de cisco 54321 usando o algoritmo de hashing SCRYPT**.

R2(config)# username ADMIN algorithm-type scrypt secret cisco 54321

**5° - Configure a linha do console usando as seguintes instruções:**

- **Defina o tempo limite executivo para 3 minutos na linha de concole**
  
- **Defina a linha do console para usar o banco de dados local para autenticação**
  
- **Após a configuração, o modo de configuração da linha de saída.**
  

R2(config)# line console 0

R2(config)# exec-timeout 3 0

R2(config-line)# login local

R2(config-line)# exit

**6° - Configure as linhas vty usando as seguintes instruções:**

- **Defina o tempo limite executivo para 3 minutos nas linhas do VTY.**
  
- **Defina as linhas VTY para usar o banco de dados local para autenticação.**
  

R2(config)# line vty 0 4

R2(config-line)# exec-timeout 3 0

R2(config-line)# login local

**7° - Volte ao modo EXEC privilegiado. Exiba a configuração-config e filtre-a para incluir apenas as linhas com o nome de usuário para verificar as configurações da conta de usuário.**

R2(config-line)# end

R2# show running-config | include username
