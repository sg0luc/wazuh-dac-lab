# Wazuh Detection as Code Lab

# Introdução
Seja bem-vindo(a) ao Wazuh Detection as Code Lab! Aqui, estarão localizados todos os arquivos de regras criadas para a ferramenta, bem como scripts, utilitários e configurações de pipelines CI/CD para a implementação automática das regras.

# Estrutura do repositório
- `README.md` - Exatamente onde estamos agora :)
- `decoders/` - Diretório com os decoders do Wazuh
- `rules/` - Diretório com os arquivos XML contendo as regras do Wazuh
- `tests/` - Diretório com logs de amostra para as regras em produção, bem como scripts e utilitários

# Estratégia de branching
- `main` (Protegida) - Branch de produção, recebe as regras validadas ao fazer merge com a branch `dev` 
- `dev` (Protegida) - Branch de homologação/pré-produção, recebem novas regras antes de ser mergeada à branch `main`
- `new-rule/*` - Branches para novas regras, podendo serem submetidas em grupos em um mesmo branch. Sempre deverão ser mergeadas à branch `dev` primeiro
- `bug-fix/*` - Branches para resolução de problemas em regras já existentes. Devem ser mergeadas à branch `main`

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
6. XML Linting - `xmllint`
    - Esta etapa só será executada se:
        - Arquivos de regras (XML) tenham sido modificados e/ou adicionados
        - A branch não for **main** nem **production**
7. Testes unitários - _if\_sid_, _description_, _mitre_, etc.
        - Busca por campos obrigatórios para o funcionamento correto das regras
8. Testes de integração - `wazuh-logtest`
    - Validação de match da lógica da regra com as amostras de log fornecidas na pasta logtest

## Etapa 3 - Deploy
9. Pull request
    - Aprovação e merge
11. Sincronização de repositórios
    - `git pull origin main`
12. Reinicialização do Wazuh Manager para aplicação das mudanças
    - `systemctl restart wazuh-manager`

# Referências
- [Wazuh ruleset as code (RaC)](https://wazuh.com/blog/wazuh-ruleset-as-code-rac/)
- [Automating Security Detection Engineering: A hands-on guide to implementing Detection as Code](https://www.amazon.com.br/dp/1837636419)
- [Detection-as-Code & CI/CD for Detection Engineering with Dennis Chow | Detection Opportunities EP 9](https://www.youtube.com/watch?v=Uw0r7lGN__Q)
- [BSidesSF 2022 - Detection-as-code: Why it works and where to start (Kyle Bailey)](https://www.youtube.com/watch?v=VaZp7A6Q9zE)
