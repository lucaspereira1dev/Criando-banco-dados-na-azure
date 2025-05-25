# Criando-banco-dados-na-azure

Criação e configuração de um Banco de Dados Azure, documentados para fins de consulta futura e como desafio proposto pela DIO no bootcamp "XP Inc. Cloud com Inteligência Artificial".
Provisionamento de Banco de Dados MySQL na Azure Utilizando o Nível Gratuito (Free Tier)

Este documento descreve o procedimento técnico para o provisionamento e configuração inicial de um Banco de Dados MySQL na plataforma Microsoft Azure, com foco exclusivo na utilização de recursos elegíveis ao "Free Tier". O objetivo é fornecer um guia detalhado e reprodutível, adequado para fins acadêmicos, experimentação de serviços PaaS (Platform as a Service) e como material de referência para projetos universitários, como o desafio proposto pela DIO no bootcamp "XP Inc. Cloud com Inteligência Artificial".
1. Pré-requisitos

    Conta Azure com Nível Gratuito (Free Tier) Ativo: Necessária para acesso aos serviços e benefícios gratuitos. Detalhes em azure.microsoft.com/free.
    Familiaridade com o Portal Azure: Conhecimento básico da interface de gerenciamento.
    Cliente MySQL:
        Para Linux/macOS: mysql-client (geralmente nativo ou facilmente instalável).
        Para Windows: MySQL Workbench ou cliente de linha de comando MySQL.
    Conceitos Básicos de Banco de Dados e Redes.

2. Metodologia de Provisionamento do Banco de Dados

O processo de provisionamento será conduzido integralmente através do Portal Azure.
2.1. Estabelecimento da Conta Azure

A criação de uma conta no Nível Gratuito da Azure é o passo inicial. Este processo envolve:

    Navegar até azure.microsoft.com e selecionar a opção de criação de conta gratuita.
    Prover as informações solicitadas, incluindo um método de verificação de identidade (cartão de crédito, que não será cobrado dentro dos limites do nível gratuito).
    Após a confirmação, realizar login no Portal Azure (portal.azure.com).

2.2. Criação e Configuração do Servidor de Banco de Dados

No Portal Azure, o fluxo de criação do banco de dados é iniciado pela busca do serviço "Servidores do Banco de Dados do Azure para MySQL" e subsequente seleção de "Criar".
2.2.1. Configurações Fundamentais (Aba "Básico")

    Assinatura: Deve ser selecionada a assinatura associada ao Nível Gratuito.
    Grupo de Recursos:
        É imperativo criar um novo grupo de recursos para encapsular todos os artefatos do banco de dados, facilitando o gerenciamento e a eventual desmobilização.
        Ação: Clicar em "Criar novo".
        Nomenclatura Sugerida: rg-mysql-freetier-lab (ou similar, seguindo uma convenção de nomenclatura).
    Detalhes do Servidor:
        Nome do servidor: Um identificador único para o servidor MySQL (e.g., mysql-server-lab-01).
        Região: Selecionar uma região que ofereça explicitamente os serviços gratuitos desejados (e.g., East US, West Europe). A região Brazil South pode ser uma opção, sujeita à disponibilidade dos SKUs gratuitos para MySQL.
        Tipo de servidor: Manter Servidor flexível. Esta opção é mais flexível e geralmente oferece mais opções para o nível gratuito.
        Versão do MySQL: Selecionar a versão desejada (e.g., 8.0).
        Tipo de Carga de Trabalho:
            Para o nível gratuito, selecionar Desenvolvimento/Testes.
        Configurar servidor:
            Nível de computação + armazenamento:
                Ação: Clicar em "Configurar servidor".
                Tipo de preço: Ponto Crítico para o Nível Gratuito. Selecionar Expandir ou Burstable. Este tipo de preço é elegível para o nível gratuito.
                Tamanho de computação: Selecionar Burstable B1ms (1 vCore, 2 GiB RAM). Este é um SKU comumente incluído na oferta de 12 meses do nível gratuito.
                Armazenamento: Configurar para o tamanho mínimo permitido (e.g., 20 GiB). A oferta gratuita geralmente inclui uma cota de armazenamento SSD.
                Backup: Manter Redundância de armazenamento com redundância local (LRS).
                Clicar em "OK".
    Credenciais de administrador:
        Nome de usuário do administrador: Definir um nome de usuário (e.g., dbadmin).
        Senha do administrador: Definir uma senha robusta, aderente às políticas de complexidade do Azure.
        Confirmar senha: Repetir a senha.

2.2.2. Configuração de Rede (Aba "Rede")

    Método de Conectividade:
        Ação: Selecionar Acesso público (endereços IP permitidos). Esta é a opção mais simples para fins de laboratório e para conectar de máquinas fora da rede Azure.
        Regras de firewall:
            Adicionar endereço IP do cliente atual: Clicar nesta opção para permitir o acesso do seu endereço IP público à porta do MySQL.
            Se desejar acessar de uma Máquina Virtual Azure, adicione o IP público da VM ou configure a conectividade de rede privada. Para o Free Tier, o acesso público com regras de firewall é mais comum.

2.2.3. Configurações de Segurança e Marcas (Abas "Segurança de dados" e "Marcas")

    Segurança de dados:
        Habilitar SSL: Manter Sim para garantir a criptografia das conexões.
        Proteção Avançada contra Ameaças: Manter desabilitado para o nível gratuito, a menos que haja necessidade específica e se esteja ciente de possíveis custos.
    Marcas (Tags): Recomenda-se o uso para organização e rastreamento de custos, mesmo em ambientes de laboratório.
        Exemplos: Projeto:BootcampDIO, Ambiente:LabFreeTier, Owner:SeuNome.

2.2.4. Revisão e Criação

    Navegar para a aba "Revisar + criar".
    O Azure realizará uma validação final das configurações. Corrigir quaisquer erros apontados.
    Analisar o resumo, confirmando o tipo de preço (Burstable B1ms), armazenamento e a estimativa de custo (deve ser R$0,00 ou valor muito próximo, indicando o uso do nível gratuito).
    Clicar em "Criar".

3. Acesso ao Banco de Dados Pós-Provisionamento

Após a conclusão da implantação:

    Navegue até o recurso do servidor MySQL no Portal Azure.

    Na seção "Visão geral", localize e copie o "Nome do servidor" (Ex: mysql-server-lab-01.mysql.database.azure.com) e o "Nome do usuário do administrador do servidor" (Ex: dbadmin).

    Conexão via Cliente MySQL:
    Bash

mysql -h <nome_do_servidor> -u <nome_de_usuario_admin> -p

Substitua <nome_do_servidor> e <nome_de_usuario_admin> pelos valores obtidos no portal. Será solicitada a senha definida durante a criação.

Exemplo:
Bash

mysql -h mysql-server-lab-01.mysql.database.azure.com -u dbadmin -p

Após a conexão, você poderá executar comandos SQL para criar bancos de dados, tabelas e gerenciar seus dados.
SQL

    CREATE DATABASE myapplicationdb;
    USE myapplicationdb;
    CREATE TABLE users (id INT AUTO_INCREMENT PRIMARY KEY, name VARCHAR(255));
    INSERT INTO users (name) VALUES ('João');
    SELECT * FROM users;

4. Gerenciamento de Recursos e Otimização de Custos (Nível Gratuito)

    Parada do Servidor: Ao contrário das VMs, o servidor de Banco de Dados do Azure para MySQL (Flexible Server) não possui uma opção direta de "parar" e desalocar completamente para o Free Tier sem perder a configuração ou incorrer em custos mínimos de armazenamento. No entanto, o SKU Burstable foi projetado para custos otimizados.
    Monitoramento de Consumo: Utilize o serviço "Gerenciamento de Custos + Cobrança" no Portal Azure para acompanhar o uso dos serviços e evitar exceder os limites do nível gratuito.
    Desmobilização de Recursos: Ao concluir o uso, exclua o Grupo de Recursos criado. Esta ação removerá todos os recursos contidos (servidor MySQL, etc.), prevenindo quaisquer cobranças futuras.

5. Conclusão

Este documento detalhou o processo de provisionamento de um Banco de Dados MySQL na Azure, aderindo estritamente aos recursos do Nível Gratuito. A metodologia apresentada permite a criação de um ambiente de laboratório funcional para desenvolvimento, testes e aprendizado em computação em nuvem, sem incorrer em custos iniciais, desde que os limites da oferta gratuita sejam respeitados.
