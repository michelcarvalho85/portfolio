# Como adicionar novas demos

Crie uma pasta em `projetos/` com número e nome descritivo, por exemplo `02-nome-do-projeto/`.

Em cada pasta, inclua um `README.md` com:

1. Contexto e problema de negócio.
2. Objetivo e sua participação.
3. Tecnologias e arquitetura.
4. Imagens com dados fictícios e descrições acessíveis.
5. Decisões técnicas e aprendizados.
6. Instruções de execução, quando houver uma demo executável.
7. Resultados comprovados e limites da demonstração.

Use subpastas como `imagens/`, `sql/`, `dax/` e `dados-demo/` conforme a necessidade. Atualize a tabela no README principal e o índice de projetos com links relativos.

O `.gitignore` exclui materiais privados, credenciais, arquivos Power BI e exportações de dados por padrão. Para uma nova base fictícia revisada, adicione uma exceção específica para o arquivo que será compartilhado. O bloqueio de PBIX existe porque esse formato pode incorporar dados e conexões; sua publicação exige revisar o conteúdo antes de liberar o arquivo.

Antes de enviar alterações, confira `git status` e `git diff --cached --stat`. Adicione explicitamente os arquivos públicos do novo projeto.
