# Wazuh Detection as Code Lab

# Introdução
Seja bem-vindo(a) ao Wazuh Detection as Code Lab! Aqui, estarão localizados todos os arquivos de regras criadas para a ferramenta, bem como scripts, utilitários e configurações de pipelines CI/CD para a implementação automática das regras.

# Estrutura do repositório
- `README.md` - Exatamente onde estamos agora :)
- `decoders/` - Diretório com os decoders do Wazuh
- `rules/` - Diretório com os arquivos XML contendo as regras do Wazuh
- `tests/` - Diretório com logs de amostra para as regras em produção, bem como scripts e utilitários

# Topologia do laboratório
![Wazuh Detection as Code Lab](lab.png)

# Branches
- `new-rule/*` - Branches para novas regras, podendo serem submetidas em grupos em um mesmo branch. Sempre deverão ser mergeadas a branches de features
- `feature/*` - Branches para um conjunto de novos arquivos de regra, como novas tecnologias ou produtos. Sempre deverão ser mergeadas à branch `main`
- `bug-fix/*` - Branches para a resolução de problemas em regras já existentes. Podem ser mergeadas à branch `main` ou `feature/*`
- `dev` (Protegida) - Branch de pré-produção, recebem novas regras e arquivos primeiro
- `main` (Protegida) - Branch de produção

# Fluxo operacional padrão 

## Possíveis fontes de entrada e/ou solicitações comuns
- Solicitação de correção na regra ou grupo de regras
- Ajuste ou refinamento de regra(s) já existente(s)
- Novas regras de detecção

## Etapa 1 - Atendimento da issue
1. Criar nova branch associada à mesma categoria da issue
2. Realizar mudanças
3. Executar simulação de adversários
4. Coletar evidências e logs para teste
5. Commit & push

## Etapa 2 - CI/CD
6. Push linting - `xmllint`
    - Esta etapa só será executada se:
        - Arquivos de regras (XML) tenham sido modificados e/ou adicionados
        - A branch não for **main** nem **production**
7. Merge request
    - Testes unitários - _if\_sid_, _description_, _mitre_, etc.
        - Busca por campos obrigatórios para o funcionamento correto das regras
8. Testes de integração - `wazuh-logtest`
    - Validação de match da lógica da regra com as amostras de log fornecidas na pasta logtest

## Etapa 3 - Deploy
9. Merge request
10. Sincronização de repositórios
    - `git pull origin main`
11. Reinicialização do Wazuh Manager para aplicação das mudanças
