#Capitulo 2

## Construindo uma frente de loja com AngularJS

Vamos usar o AngularJS para construir o front-end, o angularJS é um framework muito conhecido que surgiu em 2009 e ganhou
a graça dos desenvolvedores por ser muito completo e ideal para aplicações SPA ( single page applications ), o angularJS é
um framework MVC ( model view controller ), nesse capitulo vamos ver o seguinte:

<ul>
  <li>Entendendo o AngularJS e a estrutura de pastas do cliente</li>
  <li>Projeto visual do site e-commerce</li>
  <li>Administrando produtos (CRUD)</li>
</ul>

### Entendendo o AngularJS e a estrutua de pastas do cliente.

vamos rodar o grunt com **grunt serve** ( lembrando que devemos estar com o servidor do mongo online **mongod** ), vamos
observar a estrutura de pastas criadas pelo Yeoman, temos três páginas principais sendo elas ** client **, ** server**
,** e2e **. Nesse capitulo vamos focar em client que é onde vão estar os códigos do angularJS e itens como imagens, fontes
entre outros... dentro da pasta cliente vamos ter a pasta **app** onde vão estar os códigos do angularJS dividos da seguinte
maneira:
root/client/app/cliente
<ul>
  <li>cliente.js - **Rotas**</li>
  <li>cliente.controller.js - **Controladores**</li>
  <li>cliente.controller.spec.js - **Testes**</li>
  <li>cliente.html - **Visão**</li>
  <li>cliente.css - **Folha de estilos**</li>
</ul>

#### Diretivas

As diretivas são extenssões HTML na forma de atributos, tags, classes CSS e até comentários. as diretivas são um grande 
diferencial do angularJS pois com elas conseguimos estender as funcionalidades do html e podemos até mesmo criar nossas
próprias diretivas. A diretiva ng-app é necessária para inciiar o angularJS automaticamente, ela define o modulo raiz.

##### Módulos

Os módulos são o meio preferido para organizar o código em projetos do angularJS. o angularJS fornece uma variável global
chamada angular e ela tem a função module a função module será utilizada para configurar, chamar módulos e administrar nossas
dependências, angular.module pode ser chamado como um getter ou setter:

angular.module('nomeDoModulo') -> getter
angular.module('nomeDoModulo',['dependência']) -> setter

quando o angular se deparar com o nomeDoModulo ele já vai saber que terá que executar um módulo com esse nome e deixar 
suas dependências disponíveis, o getter angular.module tem um grande número de métodos que podem ser encadeados a ele 
como config, factory, run e etc...

##### Roteamento no AngularUI

A funcionalidade de roteamento permite que o usuário tenha uma URL que reflita o estado atual da aplicação. Normalmente
uma aplicação SPA pode ter inúmeras páginas diferentes em um único e imutável URL, para contornar esse problema usaremos
um módulo chamado ui-router, que associará URLs únicos a estados específicos da aplicação, podemos ver um exemplo:

````js
angular.module('meanshopApp')
  .config(function ($stateProvider) {
    $stateProvider
      .state('main', {
        url: '/',
        templateUrl: 'app/main/main.html',
        controller: 'MainCtrl'
      });
  });
````
