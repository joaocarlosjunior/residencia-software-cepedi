# Avaliação

## Tarefas

1. **Backend e Frontend**:
    - Você já possui o código da aplicação do backend e frontend, além da configuração do Nginx (nginx.conf).
    - Sua tarefa será criar os Dockerfiles para o backend e frontend e o docker-compose.yml para orquestrar os containers, subindo toda a infraestrutura utilizando Docker.
2. **Requisitos de Nomeação dos Services**:
    - Os serviços definidos no docker-compose.yml devem obrigatoriamente ser nomeados da seguinte forma:
        - O serviço do frontend deve ser chamado de frontend.
        - O serviço do backend deve ser chamado de backend.
        - O serviço do banco de dados MySQL deve ser chamado de db.
3. **Requisitos para o Backend**:
    - O backend deverá rodar na porta 5000 e se comunicar com o banco de dados MySQL.
    - Você deverá utilizar as variáveis de ambiente:
        - DB_HOST: Host do banco de dados.
        - DB_USER: Usuário do banco de dados.
        - DB_NAME: Nome do banco de dados.
    - As senhas do banco de dados deverão ser fornecidas através de **Docker Secrets**:
        - mysql_root_password: Senha do usuário root do MySQL.
        - mysql_password: Senha do usuário padrão do MySQL.
4. **Requisitos para o Banco de Dados (MySQL 8.0)**:
    - O banco de dados MySQL 8.0 deverá ser configurado para utilizar um volume chamado db-data-idade para persistir os dados.
    - O banco de dados será acessado pelo backend para armazenar as informações dos usuários (nome e idade).
    - As senhas do MySQL serão configuradas utilizando secrets (mysql_root_password e mysql_password).
5. **Requisitos para o Frontend**:
    - O frontend será servido pelo Nginx na porta 80, onde os clientes poderão acessar a aplicação web.
    - O Nginx também deverá atuar como proxy reverso, encaminhando as requisições do frontend para o backend na porta 5000.

### Comunicação entre o Frontend e o Backend

A comunicação entre o frontend (Nginx) e o backend (Flask) será feita da seguinte maneira:

- O Nginx será configurado como um proxy reverso, o que significa que ele receberá as requisições HTTP dos clientes e redirecionará as chamadas para o backend.
- Sempre que o frontend fizer uma chamada para /api, o Nginx encaminhará a requisição para o backend Flask que está rodando na porta 5000.
- As requisições do frontend, como criação e listagem de usuários, serão redirecionadas para o backend que processa a requisição e interage com o banco de dados MySQL. O backend então retorna a resposta ao Nginx, que a envia de volta para o cliente no navegador.

### Comunicação:

- O cliente acessa a aplicação frontend em http://localhost:80.
- O frontend envia uma requisição para listar os usuários em http://localhost/api/users.
- O Nginx intercepta essa requisição e a redireciona para http://backend:5000/api/users.
- O backend processa a requisição e retorna os dados dos usuários, que são exibidos no frontend.
- O arquivo nginx.conf já foi fornecido, de forma que o nginx já deverá atuar para fazer essa comunicação, além disso, no frontend (main.js) existe uma variável denominada apiUrl que faz o mapeamento para /api/users.


### Orientações

1. **Suba a infraestrutura utilizando Docker**:
    -  Utilize Docker Compose para definir e orquestrar os containers do backend, frontend e banco de dados.
    - Certifique-se de que o backend se comunica corretamente com o banco de dados MySQL e que o frontend consegue consumir a API REST exposta pelo backend.
2. **Use Variáveis de Ambiente e Secrets**:
    - Configure as variáveis de ambiente DB_HOST, DB_USER, e DB_NAME no backend.
    - As senhas do MySQL devem ser configuradas como secrets no Docker (mysql_root_password e mysql_password).
    - O backend deverá acessar o banco de dados utilizando essas variáveis e secrets.
3. **Persistência de Dados**:
    - O banco de dados MySQL deve utilizar o volume db-data-idade para garantir a persistência dos dados entre execuções.
4. **Rede Docker**:
    - Utilize uma rede personalizada chamada idade-network para conectar todos os serviços.
5. **Portas**:
    - O backend deverá escutar na porta 5000.
    - O frontend deverá estar disponível na porta 80 para os clientes.