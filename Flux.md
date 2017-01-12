**Flux** é uma arquitetura utilizada para construir aplicações web ao lado do cliente, com foco na reutilização de código através do encapsulamento de componentes, complementando os componentes combináveis criados com ReactJS, utilizando um método de transporte de dados unidirecional.

Qual a idéia por trás disto?

Basicamente a ideia é modularizar a aplicação em blocos que contém comportamentos, os elementos são renderizados de acordo com as alterações de estado, obedecendo recursos de callback oferecidos pelo framework.

Outra coisa bacana é que o framework disponibiliza um "DOM Virtual" onde são aplicadas as mudanças de estado que refletem no DOM apresentado ao usuário.

As aplicações que utilizam flux, são divididas em 3 partes principais, o dispatcher, as stores, e as views(Componentes ReactJs).

Porém, este pattern não deve ser confundido com MVC, pois em flux existem os View-Controllers, que respondem as interações do usuário.

## Dispatchers

É o ponto central que gerencia todo o fluxo de dados em um aplicativo FLUX. É, essencialmente, um registro de callbacks para as stores. Cada store se registra e fornece uma callback. Quando o Dispatcher responde a uma ação, todas as stores do aplicativo recebem a carga de dados fornecido pela ação através das callbacks registradas.

## Stores

Contém o estado do aplicativo e lógica. Seu papel é um pouco semelhante a um model em um MVC tradicional, mas eles gerenciam o estado de muitos objetos e não instâncias de um único objeto. Também não são as mesmas coleções de Backbone. Mais do que simplesmente gerenciar uma coleção de objetos de estilo ORM, gerenciam o estado do aplicativo para um determinado domínio do aplicativo.

Por exemplo, o Facebook's Lookback Video Editor utilizou uma `TimeStore` que acompanhou a posição de tempo de reprodução e do estado de reprodução. Por outro lado, A `ImageStore` do mesmo aplicativo acompanhou uma coleção de imagens.

Como mencionado acima, uma Store se registra com o dispatcher, dando-lhe uma callback. Esta callback recebe carga de dados da ação como um parâmetro. A carga útil contém um atributo tipo, identificando o tipo da ação. Dentro da callback registrada da Store, uma instrução switch com base no tipo de ação é usado para interpretar a carga e fornecer os ganchos apropriados para métodos internos da store. Isso permite uma ação para resultar em uma atualização para o estado da store, através do despachante. Depois das stores serem atualizadas, elas transmitem um evento declarando que seu estado mudou, por isso as Views podem consultar o novo estado e atualizar-se.

## Views

ReactJs proporciona componentes combináveis ​​que precisamos para a camada view. Perto do topo da hierarquia, um tipo especial de view escuta eventos que são transmitidos pelas stores dos quais ele depende. Pode-se chamar isso de view do controlador, uma vez que fornece o código para obter os dados das stores e passar esses dados para baixo na cadeia de seus descendentes. Podemos ter uma view-controller para controlar qualquer parte significativa da página.

Quando se recebe o evento a partir da store, ele primeiro solicita os novos dados de que necessita através de métodos `getter()` público das stores. Em seguida, chama o seu próprio `setstate()` ou `ForceUpdate()`, fazendo com que o seu método `render()` de todos os seus descendentes seja executado.

Costumamos passar todo o estado da store da cadeia de pontos de vista em um único objeto, permitindo que diferentes descendentes usem o que eles precisarem. Além de manter o comportamento do controlador no topo da hierarquia, e, assim, manter nossos pontos de vista descendentes funcionalmente puros tanto quanto for possível, passando para baixo todo o estado de armazenamentos em um único objeto, que também tem o efeito de reduzir o número de adereços que precisamos gerir.

## Flux x MVC

Em comparação com o pattern MVC, podemos dizer que as stores seriam próximas a camada model, mas é difícil comparar a camada view, pois ela é o próprio controller da aplicação em flux. Diferentemente do controller do MVC que aciona e é acionado pela view, as view-controllers acionam diretamente as suas stores, enquanto o dispatcher trata das dependências entre as stores.

Abaixo o ciclo de aplicativos Flux, demonstrando que os dados fluem em uma única direção e permanecem em ciclos associados a interatividade do usuário.

Fonte: http://pt.stackoverflow.com/questions/27167/como-funciona-a-arquitetura-flux
