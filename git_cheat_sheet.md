## GITHUB CHEAT SHEET




### Requisitos

1. Cria uma conta no Github
2. Cria o repositório para seu projeto
3. Clone o repósitório em sua máquina, utilizando o modo de conexão ssh

### Criando uma Chave SSH

`ssh-keygen -t rsa -C meu_email@email.com -f nome_da_chave`

Opcionalmente será solicitado a criação de uma senha de uso. Como boa prática digite e repita uma senha pessoal.
Serão gerados duas chaves, uma privada e uma pública (.pub)

Verifique o valor de sua chave publica com:

`cat ~/.ssh/nome_da_chave.pub`

Copie o conteúdo da chave e registre no campo de chaves ssh de sua conta no GitHub.

Para testar a sua conexão com o github digite:

`ssh -T git@github.com`

Se tudo estiver configurado corretamente será exibido uma mensagem de boas vindas e sucesso.

`ssh-keygen -t rsa -C meu_email@email.com -f nome_da_chave`


**Clone o repositório do Github para a sua máquina**

1.Vá até a pasta onde desejar armazenar a cópia na sua máquina
2. Clone o repositório usando 

`git clone git@git-link-para-repositorio`