Descrição detalhada do código:

1. Provider
provider "aws": Define que a infraestrutura será criada na AWS, especificando a região us-east-1.
2. Variáveis
variable "projeto": Variável que armazena o nome do projeto, com um valor padrão de "VExpenses".
variable "candidato": Variável que armazena o nome do candidato, com um valor padrão de "SeuNome".
3. Recursos
-Chave Privada TLS

resource "tls_private_key" "ec2_key": Gera uma chave privada RSA de 2048 bits, que será utilizada para autenticação na instância EC2.
-Par de Chaves AWS

resource "aws_key_pair" "ec2_key_pair": Cria um par de chaves na AWS, utilizando a chave pública gerada anteriormente. O nome da chave é formatado com o nome do projeto e do candidato.
-VPC (Virtual Private Cloud)

resource "aws_vpc" "main_vpc": Cria uma VPC com um bloco CIDR de 10.0.0.0/16, habilitando suporte a DNS e nomes de host.
-Subnet

resource "aws_subnet" "main_subnet": Cria uma sub-rede dentro da VPC com um bloco CIDR de 10.0.1.0/24 na zona de disponibilidade us-east-1a.
-Gateway de Internet

resource "aws_internet_gateway" "main_igw": Cria um gateway de internet associado à VPC, permitindo acesso à internet para os recursos dentro dela.
-Tabela de Rotas

resource "aws_route_table" "main_route_table": Define uma tabela de rotas para a VPC, com uma rota que direciona todo o tráfego (0.0.0.0/0) para o gateway de internet.
-Associação de Tabela de Rotas

resource "aws_route_table_association" "main_association": Associa a tabela de rotas criada à sub-rede.
-Grupo de Segurança

resource "aws_security_group" "main_sg": Cria um grupo de segurança que permite conexões SSH (porta 22) de qualquer lugar e permite todo o tráfego de saída.
-AMI (Amazon Machine Image)

data "aws_ami" "debian12": Busca a imagem mais recente do Debian 12, filtrando por nome e tipo de virtualização.
Instância EC2

resource "aws_instance" "debian_ec2": Cria uma instância EC2 usando a AMI Debian 12, tipo t2.micro, associada à sub-rede e ao par de chaves criado. A instância tem um volume raiz de 20 GB e um script de inicialização que atualiza e faz upgrade do sistema.
4. Saídas
output "private_key": Exibe a chave privada gerada para acesso à instância EC2, marcada como sensível.
output "ec2_public_ip": Exibe o endereço IP público da instância EC2 criada.
Resumo:
Este código Terraform automatiza a criação de uma infraestrutura básica na AWS, incluindo uma VPC, sub-rede, gateway de internet, tabela de rotas, grupo de segurança, e uma instância EC2 com Debian 12. As variáveis permitem personalizar o nome do projeto e do candidato, enquanto as saídas fornecem informações essenciais para acessar a instância.






