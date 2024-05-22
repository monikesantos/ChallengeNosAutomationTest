# ChallengeNosAutomationTest
Challenge Nos Automation Test / Desafio Processo Seletivo NOS Portugal 
Este documento fornece instruções detalhadas sobre como executar testes no Postman para a API https://gorest.co.in/public/v2/todos. Os testes incluem a criação de uma coleção, a validação do schema da resposta, a verificação do status e a validação do formato da data.

Pré-requisitos
Postman:  Postman instalado em sua máquina.

1. Criar Coleção e Requisição
Abra o Postman
Clique em New no canto superior esquerdo e selecione Collection.
Nomeie a coleção, por exemplo, GoRest Todos Tests.
Clique em Add Request e nomeie a requisição como Get Todos.
Defina o método HTTP como GET.
Insira a URL: https://gorest.co.in/public/v2/todos.
Clique em Save para salvar a requisição na coleção.

3. Adicionar Testes à Requisição
Após criar a requisição, adicione os seguintes testes no Postman:

a. Validar Schema da Resposta
Na aba Tests da sua requisição Get Todos, adicione o seguinte código para validar o schema da resposta:

pm.test("Validar schema da resposta", function () {
    var schema = {
        "type": "array",
        "items": {
            "type": "object",
            "properties": {
                "id": {"type": "integer"},
                "user_id": {"type": "integer"},
                "title": {"type": "string"},
                "due_on": {"type": "string", "format": "date-time"},
                "status": {"type": "string"}
            },
            "required": ["id", "user_id", "title", "due_on", "status"]
        }
    };
    pm.response.to.have.jsonSchema(schema);
});


b. Validação de Status "Completed"
Ainda na aba Tests, adicione o seguinte código para verificar se todos os resultados têm o status "completed":

pm.test("Todos os resultados têm status completed", function () {
    let jsonData = pm.response.json();
    jsonData.forEach(item => {
        pm.expect(item.status).to.equal("completed");
    });
});


c. Validação do Formato do Campo due_on
Adicione o seguinte código na aba Tests para validar o formato do campo due_on:


pm.test("Validar formato do campo due_on", function () {
    let jsonData = pm.response.json();
    jsonData.forEach(item => {
        pm.expect(item.due_on).to.match(/^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}\.\d{3}Z$/);
    });
});


3. Executar os Testes
Após adicionar os testes, clique em Send para enviar a requisição.
Verifique a aba Tests na resposta para ver os resultados dos testes.
Certifique-se de que todos os testes foram aprovados.
Exemplo de Resultado
Se todos os testes forem aprovados, será exibido algo assim na aba Tests:

✓ Validar schema da resposta
✓ Todos os resultados têm status completed
✓ Validar formato do campo due_on


Caso algum teste falhe, uma mensagem de erro será exibida com detalhes sobre a falha.
Para mais informações sobre o Postman e escrever testes, consulte a documentação oficial do Postman.
