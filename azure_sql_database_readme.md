# Criação de Banco de Dados SQL no Microsoft Azure

## Visão Geral

Este guia detalha o processo completo para criar um Banco de Dados SQL no Microsoft Azure, cobrindo desde os pré-requisitos até configurações avançadas. O Azure SQL Database é um serviço PaaS (Platform as a Service) que oferece alta disponibilidade, escalabilidade automática e gerenciamento simplificado.

## Tipos de Implantação SQL no Azure

### Azure SQL Database
- **Banco único**: Um banco isolado com recursos dedicados
- **Pool elástico**: Múltiplos bancos compartilhando recursos
- **Melhor para**: Aplicações modernas na nuvem, SaaS

### Instância Gerenciada do SQL
- **Compatibilidade**: Quase 100% com SQL Server on-premises
- **Melhor para**: Migração lift-and-shift, aplicações legadas

### SQL Server em VMs
- **Controle total**: Acesso completo ao SO e SQL Server
- **Melhor para**: Configurações customizadas, aplicações específicas

## Pré-requisitos

- Conta ativa do Microsoft Azure
- Assinatura com créditos ou método de pagamento
- Permissões para criar recursos (SQL DB Contributor ou superior)
- Conhecimento básico de SQL e administração de bancos

## Processo de Criação - Azure SQL Database

### 1. Acesso ao Portal
- Acesse [portal.azure.com](https://portal.azure.com)
- Faça login com suas credenciais
- Clique em **"Criar um recurso"** (+)

### 2. Seleção do Serviço SQL
- Procure por **"SQL Database"** ou **"Base de dados SQL"**
- Clique em **"SQL Database"**
- Selecione **"Criar"** para iniciar o assistente

### 3. Configurações Básicas

#### Detalhes do Projeto
- **Assinatura**: Selecione a assinatura desejada
- **Grupo de Recursos**: 
  - Criar novo ou usar existente
  - Exemplo: `rg-database-producao`
  - Facilita organização e gerenciamento de custos

#### Detalhes do Banco de Dados
- **Nome do Banco**: Nome único dentro do servidor
  - Deve ter entre 1-128 caracteres
  - Não pode conter: `<>*%&:\\/? ou espaços`
  - Exemplo: `db-ecommerce-prod`

- **Servidor**: Configurar servidor SQL lógico
  - **Criar novo servidor** (primeira vez) ou
  - **Usar servidor existente**

### 4. Configuração do Servidor SQL

#### Detalhes do Servidor (se criando novo)
- **Nome do Servidor**: Nome globalmente único
  - Formato: `nome-servidor.database.windows.net`
  - Exemplo: `sql-empresa-prod-2024`

- **Localização**: Região geográfica
  - Escolha próxima aos usuários
  - Brazil South ou Brazil Southeast para usuários no Brasil

#### Autenticação do Servidor
- **Método de Autenticação**:
  - **Usar autenticação SQL**: Usuário/senha tradicional
  - **Usar Azure AD**: Integração com Active Directory
  - **Usar ambos**: Flexibilidade máxima (recomendado)

- **Logon do Administrador do Servidor**:
  - Nome do usuário admin (não pode ser 'admin', 'administrator', 'sa')
  - Exemplo: `sqladmin`

- **Senha**: 
  - Mínimo 8 caracteres, máximo 128
  - Deve conter 3 dos 4: maiúscula, minúscula, número, caractere especial
  - Evite caracteres: `"` `'` ``` `\` `/`

### 5. Configuração de Computação e Armazenamento

#### Modelo de Compra
- **vCore**: Baseado em núcleos virtuais
  - Maior controle sobre recursos
  - Opção Híbrida com Azure Hybrid Benefit
- **DTU**: Database Transaction Units (mais simples)
  - Combinação de CPU, memória e I/O
  - Ideal para iniciantes

#### Camadas de Serviço (vCore)

**Uso Geral (General Purpose)**
- Armazenamento remoto com cache local
- Boa para maioria das cargas de trabalho
- Latência: 5-7ms para leitura, 5-10ms para escrita

**Comercialmente Crítico (Business Critical)**
- Armazenamento SSD local
- Alta performance e disponibilidade
- Latência: 1-2ms
- Réplicas legíveis incluídas

**Hiperescala (Hyperscale)**
- Altamente escalável (até 100TB)
- Crescimento rápido de dados
- Backup e restauração instantâneos

#### Configuração de Hardware
- **Geração**: Gen5 (mais recente)
- **vCores**: 2, 4, 8, 16, 24, 32, 40, 80
- **Memória**: Automaticamente alocada
- **Armazenamento**: 
  - Geral: 5GB a 4TB
  - Crítico: 5GB a 4TB  
  - Hiperescala: até 100TB

#### Configuração DTU (alternativa)
- **Básico**: 5 DTUs, até 2GB
- **Padrão**: 10-3000 DTUs, até 1TB
- **Premium**: 125-4000 DTUs, até 4TB

### 6. Configurações de Backup

#### Redundância de Backup
- **Localmente Redundante (LRS)**: 
  - 3 cópias na mesma região
  - Menor custo
- **Zona-Redundante (ZRS)**: 
  - Cópias em zonas diferentes na região
  - Maior disponibilidade
- **Geo-Redundante (GRS)**: 
  - Cópias em região pareada
  - Proteção contra desastres regionais

#### Retenção de Backup
- **Point-in-time restore**: 7-35 dias (configurável)
- **Retenção longa**: Até 10 anos
- **Backups automáticos**: Não requer configuração

### 7. Configurações de Rede

#### Conectividade
- **Público (todas as redes)**: Acesso pela internet
- **Público (IPs selecionados)**: Lista de IPs permitidos
- **Ponto de extremidade privado**: Acesso via rede privada

#### Firewall e Redes Virtuais
- **Permitir serviços do Azure**: Acesso de outros serviços Azure
- **Regras de Firewall**: 
  - Adicionar IP atual
  - Definir faixas de IP
  - Exemplo: `200.201.202.0/24`

#### Proteção Avançada contra Ameaças
- **Microsoft Defender para SQL**: 
  - Detecção de anomalias
  - Avaliação de vulnerabilidades
  - Classificação de dados

### 8. Configurações Adicionais

#### Fonte de Dados
- **Nenhuma**: Banco vazio
- **Exemplo (AdventureWorksLT)**: Dados de exemplo
- **Backup**: Restaurar de backup existente
- **Ponto de restauração**: Restaurar de momento específico

#### Ordenação do Banco
- **Padrão**: `SQL_Latin1_General_CP1_CI_AS`
- **Personalizada**: Para idiomas específicos
- **Importante**: Não pode ser alterada após criação

#### Configurações de Recuperação
- **Geo-replicação**: Réplicas em outras regiões
- **Failover automático**: Grupo de failover
- **Backup geo-restaurável**: Habilitado por padrão

### 9. Tags e Metadados

#### Tags Recomendadas
- `Ambiente: Producao`
- `Projeto: ECommerce`
- `Owner: team-database@empresa.com`
- `CostCenter: TI-001`
- `BackupRequired: Daily`

### 10. Revisão e Criação
- **Validação**: Azure valida todas as configurações
- **Resumo**: Revise configurações e estimativa de custo
- **Termos**: Aceite os termos de uso
- **Criar**: Confirme a criação (processo leva alguns minutos)

## Configurações Pós-Criação

### 1. Configuração de Firewall
```sql
-- Adicionar regra de firewall via T-SQL
EXECUTE sp_set_firewall_rule N'MinhaRegra', '192.168.1.1', '192.168.1.10';
```

### 2. Criação de Usuários
```sql
-- Criar usuário no banco
CREATE USER [usuario1] WITH PASSWORD = 'SenhaForte123!';

-- Conceder permissões
ALTER ROLE db_datareader ADD MEMBER [usuario1];
ALTER ROLE db_datawriter ADD MEMBER [usuario1];
```

### 3. Configuração de Conexão
```csharp
// String de conexão .NET
"Server=tcp:meuservidor.database.windows.net,1433;Initial Catalog=meubanco;Persist Security Info=False;User ID=usuario;Password=senha;MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;"
```

### 4. Monitoramento Inicial
- **Query Performance Insight**: Análise de consultas
- **Intelligent Insights**: Recomendações automáticas  
- **Alertas**: Configurar para CPU, DTU, conexões

## Métodos Alternativos de Criação

### Azure CLI
```bash
# Criar grupo de recursos
az group create --name myResourceGroup --location "Brazil South"

# Criar servidor SQL
az sql server create --name meuservidor --resource-group myResourceGroup --location "Brazil South" --admin-user sqladmin --admin-password SenhaForte123!

# Criar banco de dados
az sql db create --resource-group myResourceGroup --server meuservidor --name meubanco --service-objective S0
```

### Azure PowerShell
```powershell
# Criar grupo de recursos
New-AzResourceGroup -Name "myResourceGroup" -Location "Brazil South"

# Criar servidor SQL
New-AzSqlServer -ResourceGroupName "myResourceGroup" -ServerName "meuservidor" -Location "Brazil South" -SqlAdministratorCredentials $(Get-Credential)

# Criar banco de dados
New-AzSqlDatabase -ResourceGroupName "myResourceGroup" -ServerName "meuservidor" -DatabaseName "meubanco" -RequestedServiceObjectiveName "S0"
```

### ARM Template
```json
{
  "type": "Microsoft.Sql/servers/databases",
  "apiVersion": "2021-11-01",
  "name": "[concat(parameters('serverName'), '/', parameters('databaseName'))]",
  "location": "[parameters('location')]",
  "properties": {
    "collation": "SQL_Latin1_General_CP1_CI_AS",
    "maxSizeBytes": "2147483648",
    "requestedServiceObjectiveName": "S0"
  }
}
```

## Tipos de Configuração Avançada

### 1. Pools Elásticos
- Compartilhamento de recursos entre múltiplos bancos
- Otimização de custos para padrões de uso variáveis
- Configuração de eDTU ou vCore compartilhado

### 2. Geo-Replicação Ativa
```sql
-- Configurar réplica secundária
ALTER DATABASE meuBanco 
ADD SECONDARY ON SERVER 'servidor-secundario'
WITH (ALLOW_CONNECTIONS = READ_ONLY);
```

### 3. Grupos de Failover
- Failover automático entre regiões
- Endpoint único para aplicação
- RTO < 1 hora, RPO < 5 min

### 4. Auditoria e Conformidade
```sql
-- Habilitar auditoria
ALTER DATABASE SCOPED CONFIGURATION SET AUDIT_GUARD = ON;
```

## Segurança e Boas Práticas

### Segurança
- **Always Encrypted**: Criptografia de dados sensíveis
- **Transparent Data Encryption (TDE)**: Criptografia em repouso
- **Dynamic Data Masking**: Mascaramento de dados sensíveis
- **Row Level Security**: Controle de acesso por linha

### Autenticação
- Prefira Azure AD sobre SQL Authentication
- Use Managed Identity para aplicações Azure
- Implemente MFA para administradores
- Rotacione senhas regularmente

### Rede
- Use Private Endpoints para conectividade segura
- Configure NSGs restritivos
- Monitore tentativas de conexão suspeitas
- Desabilite acesso público quando possível

### Backup e Recuperação
- Teste restaurações regulamente
- Configure retenção adequada ao negócio
- Documente procedimentos de disaster recovery
- Use geo-replicação para dados críticos

## Otimização de Performance

### Indexação
```sql
-- Criar índice otimizado
CREATE NONCLUSTERED INDEX IX_Customer_Email 
ON Customers (Email)
INCLUDE (FirstName, LastName);
```

### Query Store
```sql
-- Habilitar Query Store
ALTER DATABASE meubanco SET QUERY_STORE = ON;
```

### Configurações de Database
```sql
-- Configurar compatibilidade
ALTER DATABASE meubanco SET COMPATIBILITY_LEVEL = 150;

-- Habilitar otimizações automáticas
ALTER DATABASE meubanco SET AUTOMATIC_TUNING (FORCE_LAST_GOOD_PLAN = ON);
```

## Monitoramento e Alertas

### Métricas Importantes
- **DTU/CPU Percentage**: Utilização de recursos
- **Data IO Percentage**: Throughput de I/O
- **Log IO Percentage**: Utilização de log
- **Sessions Count**: Número de conexões
- **Deadlocks**: Quantidade de deadlocks

### Configuração de Alertas
- CPU > 80% por mais de 5 minutos
- DTU > 90% por mais de 3 minutos
- Conexões > 80% do limite
- Espaço de armazenamento > 85%

### Ferramentas de Monitoramento
- **Azure Monitor**: Métricas e logs centralizados
- **Query Performance Insight**: Análise de consultas
- **SQL Analytics**: Monitoramento avançado
- **Azure SQL Analytics**: Dashboard personalizado

## Gestão de Custos

### Otimização de Recursos
- **Auto-pause**: Para desenvolvimento/teste
- **Reserved Capacity**: Desconto para uso previsível  
- **Elastic Pools**: Compartilhamento eficiente
- **Serverless**: Computação sob demanda

### Monitoramento de Custos
- Use Azure Cost Management
- Configure alertas de orçamento
- Monitore DTU/vCore utilization
- Revise sizing regularmente

### Estratégias de Economia
- Use Azure Hybrid Benefit (licenças on-premises)
- Considere instâncias reservadas
- Automatize scale up/down baseado em agenda
- Desative recursos não utilizados

## Migração de Dados

### Ferramentas de Migração
- **Azure Database Migration Service**: Migração online
- **Data Migration Assistant**: Avaliação e migração
- **SqlPackage**: Import/export de BACPAC
- **Azure Data Factory**: ETL e migração de dados

### Processo de Migração
```bash
# Export database to BACPAC
SqlPackage.exe /a:Export /sscs:"conexao-origem" /tf:"database.bacpac"

# Import BACPAC to Azure SQL
SqlPackage.exe /a:Import /tscs:"conexao-azure" /sf:"database.bacpac"
```

## Solução de Problemas

### Problemas Comuns
- **Timeout de Conexão**: Verificar firewall e NSG
- **Performance Baixa**: Analisar Query Store e índices
- **Espaço Insuficiente**: Verificar configuração de storage
- **DTU/vCore Alto**: Otimizar consultas ou escalar

### Logs e Diagnósticos
```sql
-- Verificar conexões ativas
SELECT * FROM sys.dm_exec_sessions WHERE is_user_process = 1;

-- Top consultas por CPU
SELECT TOP 10 * FROM sys.dm_exec_query_stats 
ORDER BY total_worker_time DESC;
```

### Ferramentas de Diagnóstico
- **Activity Monitor**: Atividade em tempo real
- **Extended Events**: Monitoramento detalhado
- **Dynamic Management Views**: Informações do sistema
- **Azure SQL Insights**: Análise inteligente
