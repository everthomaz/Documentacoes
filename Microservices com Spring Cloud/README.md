# Microservices com Spring Cloud: Circuit Breaker, Hystrix e API Gateway


&nbsp;

### Spring Tools 4 for Eclipse:
[Clique aqui para baixar o Spring Tools 4 for Eclipse](https://spring.io/tools)


### MariaDB Server:
[Clique aqui para baixar o MariaDB Server](https://mariadb.org/download)



##### Circuit Breaker e Fallback:
Circuit Breaker: Permite definir um tempo máximo para responder uma requisição.
Fallback: Permite configurar a chamada de algum método ou procedimento para tratar a requisição que foi interrompida devolvendo alguma resposta mais amigável ao usuário.
Hystrix: Ferramenta da Netflix para configurar o Circuit Breaker e o Fallback.


Pergunta: É possível forçar que uma requisição seja cancelada após algum tempo, utilizando a técnica de Timeout, onde definimos um tempo máximo de processamento daquela requisição. Qual a vantagem do Circuit Breaker, em comparação ao uso de Timeout?
Resposta: Possui a vantagem principal de fechar o circuito, evitando que uma requisição com alto índice de falhas seja executada, até que o microsserviço volte a operar dentro dos parâmetros aceitáveis.
Explicação: Alternativa correta! O Circuit Breaker tem como funcionalidade principal a análise das requisições anteriores, para decidir se deve parar de repassar as requisições vindas do cliente para um microsserviço com problemas de performance. Enquanto o circuito está fechado, o Hystrix continua tentando a cada 5 segundos, com o objetivo de verificar se o servidor voltou a funcionar normalmente.


Pergunta: A capacidade que o Hystrix tem com Circuit Breaker pode ser aprimorada pelo Fallback em qual circunstância?
Resposta: Quando temos a possibilidade de criar uma funcionalidade de tratamento para uma execução que foi "cortada" pelo Circuit Breaker
Explicação: Alternativa correta! O Circuit Breaker implementado pelo Hystrix executa o processamento em uma thread separada. Quando o tempo limite é excedido, o Hystrix mata a execução da thread e, caso configurado, repassa a execução para o método de Fallback, de forma que este possa implementar livremente um tratamento de erro.


##### Bulkhead com Hystrix
Pergunta: Quando temos microsserviços, isso significa que podemos escalar a nossa aplicação horizontalmente, ou seja, subir mais máquinas para ter várias instâncias e recursos de hardware disponíveis para estas. Além disso, podemos ter várias threads dentro do mesmo microsserviço. Com Bulkhead, ganhamos mais uma funcionalidade de processamento paralelo, que nos traz qual vantagem?
Resposta: Poder agrupar e alocar grupos de threads para processamentos diferentes. Dessa forma, uma das chamadas de um microsserviço, que sofre lentidão por causa de uma integração com problema de performance, não indisponibiliza todas as outras chamadas do mesmo microsserviço
Explicação: Alternativa correta! Precisamos implementar um microsserviço tolerante a falhas, resiliente a integrações defeituosas e capaz de não indisponibilizar toda a aplicação por causa de uma única funcionalidade.


##### API Gateway com Spring Zuul
Pergunta: Para que os nossos microsserviços se comuniquem entre si, eles fazem o uso do Eureka, tanto para se disponibilizarem como para descobrir instâncias de outros microsserviços. Qual a necessidade do Api Gateway, já que os nossos microsserviços se conhecem?
Resposta: Para que os usuários consigam acessar as funcionalidades implementadas em vários microsserviços, sem que esses tenham a inteligência de saber como encontrar os microsserviços
Explicação: Alternativa correta! Uma aplicação rodando no navegador, ou mesmo em um aplicativo móvel, não deveria ter a inteligência de se comunicar com o Eureka, nosso Service Discovery, para descobrir as instâncias disponíveis. Aliás, faz sentido expor o Eureka na internet? Acho que não.

Pergunta: Fizemos uma integração entre o Zuul e o Eureka Server. O que ganhamos com essa integração?
Resposta: O Zuul utiliza o Eureka para conhecer as instâncias dos microsserviços e, usando o Ribbon, fazer o balanceamento de carga das requisições dos usuários

##### Feign interceptor
Utilizamos o Feign para repassar o token do usuário nas chamadas para os microsserviços Fornecedor e Transportador.


##### Conclusão:
Terminamos mais um treinamento. Aqui nesse treinamento nós nos preocupamos mais com a questão de integração com os microsserviços, no que se refere à segurança, a recuperação de falhas e performance. Vimos algumas técnicas como circuit-breaker que corta uma requisição que está demorando muito, para não deixar o nosso microsserviços sem recursos de threads.
Vimos o fallbackmethod em que podemos preparar uma resposta melhor para o usuário. O bulkhead que veio da ideia de navio na Engenharia Naval, que separa o casco de navio em sessões, de forma que se uma furar o navio todo não afunde, porque só aquela sessão vai se encher de água. Então se temos 40 threads disponíveis e muitos usuários acessando um recurso com problema, todas essas threads vão ficar disponíveis nas requisições para especificamente para essas requisições com problema.
Então fazemos o seguinte: vamos pegar a requisição, por exemplo de consulta, e dar 20 threads para essa requisição. Requisição de compra tem mais 20 threads. Então se algum desses dois endpoints estiverem sofrendo algum problema de performance, um não irá afetar o outro, que já tem threads específicas para ele. Além disso a gente viu a questão de fazer transação de certa forma.
Uma requisição, uma transação que dá um problema no meio do caminho, como é que nós tratamos isso? Nós salvamos o nosso processamento. Fomos salvando os estados em que ele ia mudando, inspirado na ideia de você gerando eventos do que está acontecendo com aquela requisição do usuário.
E aí você sabendo o que aconteceu, você consegue desfazer eu pedi para refazer o pedido, e a ideia é implementar métodos que permitam que isso aconteça. Além disso, para colocar nossa aplicação na internet, vimos a importância do API Gateway, que é um ponto central de requisição para nossa aplicação, inclusive pode ser aplicado filtros de segurança.
Então temos uma aplicação separada em microsserviços que estão criando várias instâncias diferentes de cada um deles, nosso usuário não tem como saber o IP de cada um, o endereço de cada um. Então nós expomos uma API Gateway, onde o usuário faz a requisição para essa aplicação e passando no path qual é o serviço que ele quer chamar.
Através do Path ele sabe qual microsserviço essa requisição tem que cair, tem que ser redirecionada. E como ele está ligado no Eureka ele consegue as informações de todas as instâncias daquele microsserviço e via Client Side Load Balancer, ele consegue rotacionar essas requisições.
Além disso implementamos a parte de autenticação e autorização com servidor de autenticação gerado um token com a tecnologia Alf2, integrando os microsserviços, o próprio API Gateway está integrado também porque ele aprendeu a repassar o token de autorização.
A loja também, usando o FeignClient, aprendeu a repassar o token nas requisições dela e montamos toda uma solução, conseguimos levantar todas as principais ferramentas necessárias para fazer uma aplicação implementada em microsserviços. Até a próxima.
