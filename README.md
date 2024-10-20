Descrição detalhada do código:

1. Provider
provider "aws": Define que a infraestrutura será criada na AWS, especificando a região us-east-1.
2. Variáveis
variable "projeto": Variável que armazena o nome do projeto, com um valor padrão de "VExpenses".
variable "candidato": Variável que armazena o nome do candidato, com um valor padrão de "SeuNome".
3. Recursos

-Chave Privada TLS:

resource "tls_private_key" "ec2_key": Gera uma chave privada RSA de 2048 bits, que será utilizada para autenticação na instância EC2.

-Par de Chaves AWS:

resource "aws_key_pair" "ec2_key_pair": Cria um par de chaves na AWS, utilizando a chave pública gerada anteriormente. O nome da chave é formatado com o nome do projeto e do candidato.

-VPC (Virtual Private Cloud):

resource "aws_vpc" "main_vpc": Cria uma VPC com um bloco CIDR de 10.0.0.0/16, habilitando suporte a DNS e nomes de host.

-Subnet:

resource "aws_subnet" "main_subnet": Cria uma sub-rede dentro da VPC com um bloco CIDR de 10.0.1.0/24 na zona de disponibilidade us-east-1a.

-Gateway de Internet:

resource "aws_internet_gateway" "main_igw": Cria um gateway de internet associado à VPC, permitindo acesso à internet para os recursos dentro dela.

-Tabela de Rotas:

resource "aws_route_table" "main_route_table": Define uma tabela de rotas para a VPC, com uma rota que direciona todo o tráfego (0.0.0.0/0) para o gateway de internet.

-Associação de Tabela de Rotas:

resource "aws_route_table_association" "main_association": Associa a tabela de rotas criada à sub-rede.
-Grupo de Segurança

resource "aws_security_group" "main_sg": Cria um grupo de segurança que permite conexões SSH (porta 22) de qualquer lugar e permite todo o tráfego de saída.
-AMI (Amazon Machine Image)

data "aws_ami" "debian12": Busca a imagem mais recente do Debian 12, filtrando por nome e tipo de virtualização.
Instância EC2

resource "aws_instance" "debian_ec2": Cria uma instância EC2 usando a AMI Debian 12, tipo t2.micro, associada à sub-rede e ao par de chaves criado. A instância tem um volume raiz de 20 GB e um script de inicialização que atualiza e faz upgrade do sistema.

4. Saídas:
   
output "private_key": Exibe a chave privada gerada para acesso à instância EC2, marcada como sensível.

output "ec2_public_ip": Exibe o endereço IP público da instância EC2 criada.



Resumo:
Este código Terraform automatiza a criação de uma infraestrutura básica na AWS, incluindo uma VPC, sub-rede, gateway de internet, tabela de rotas, grupo de segurança, e uma instância EC2 com Debian 12. As variáveis permitem personalizar o nome do projeto e do candidato, enquanto as saídas fornecem informações essenciais para acessar a instância.



Melhorias aplicadas no código main.tf: 

Utilizei uma chave RSA de 4096 bits que oferece uma segurança criptográfica mais robusta em comparação com uma chave de 2048 bits.

Restringi o acesso SSH a um endereço IP específico para reduzir a superfície de ataque, aumentando a segurança. O primeiro código é muito mais vulnerável, pois permite acesso SSH globalmente, expondo a instância a potenciais ataques.

A instalação e configuração do Nginx foi feita utilizando o 'user_data', tonando assim a instância mais funcional, pois você está preparando o ambiente para servir aplicações web, enquanto o primeiro código não faz essa configuração adicional.

Implementei uma política IAM  que adiciona uma camada de segurança, garantindo que a instância EC2 tenha permissões adequadas para acessar serviços da AWS. Sem a política IAM a instância fica sem controle de permissões, o que pode levar a riscos de segurança e problemas de acesso desnecessário a outros serviços.

Por fim, a última implementação feita foi adicionar um Bucket S3 com criptrografia KMS. Adicionar criptografia KMS aos dados armazenados no Amazon S3 é super importante para garantir a segurança e a confidencialidade das informações. Isso ajuda a proteger dados sensíveis contra acessos não autorizados e a atender requisitos de conformidade com regulamentações como GDPR e HIPAA. Por isso, excolhi adicionar o s3 juntammente com criptografia KMS para a proteção de dados sensíveis.


Documentação Completa para Reproduzir a Infraestrutura AWS com Terraform:

- Pré-requisitos

Antes de começar, eu preciso ter os seguintes itens configurados:

Conta AWS: Preciso ter uma conta da AWS ativa.

AWS CLI: Vou instalar e configurar a AWS Command Line Interface (CLI) com minhas credenciais.

Terraform: Vou instalar a versão mais recente do Terraform em minha máquina local. Posso baixar o Terraform aqui.

Editor de Texto: Usarei um editor de texto ou IDE ( Visual Studio Code ) para editar os arquivos Terraform.

- Passo a Passo
1. Configuração do Ambiente:
Crio um diretório para o projeto,
Crio um arquivo de configuração Terraform,
Crio um arquivo chamado main.tf,

2. Personalização das Variáveis:
Abro o arquivo main.tf e localizo as variáveis projeto e candidato. Posso personalizá-las conforme necessário:
   
variable "projeto" {
  default = "NomeDoMeuProjeto"
}

variable "candidato" {
  default = "MeuNome"
}


3. Inicialização do Terraform:
No terminal, dentro do diretório do projeto, executo o seguinte comando para inicializar o Terraform:

'terraform init'

4. Planejamento da Infraestrutura
Executo o comando abaixo para visualizar o que o Terraform irá criar:

'terraform plan'

Revisarei a saída para garantir que tudo esteja conforme o esperado.

5. Aplicação da Configuração
Para criar a infraestrutura, executo o seguinte comando:

'terraform apply'

O Terraform irá solicitar minha confirmação. Digito yes para prosseguir.

6. Acesso à Instância EC2
Após a aplicação bem-sucedida, o Terraform fornecerá a chave privada para acessar a instância EC2 e o endereço IP público.


7. Limpeza da Infraestrutura
Se eu quiser remover toda a infraestrutura criada, executo:

'terraform destroy'

O Terraform solicitará minha confirmação. Digito yes para prosseguir.





