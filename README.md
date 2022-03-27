# Aluraframe

Roteiro de acompanhamento do conteúdo do curso ["Conhecendo o Browser e Padrões de projeto"](https://www.alura.com.br/conteudo/javascript-es6-orientacao-a-objetos-parte-1) pelo professor Flávio Almeida.

O projeto, em sua primeira fase, consiste em uma listagem de negociações, preenchida com as seguintes informações sobre cada uma: Data, Quantidade e Valor.

Nas palavras do instrutor: 'Extraindo o melhor que é oferecido do ES6, usando Orientação a Objetos e programação funcional em conjunto, simulando em alguns pontos o comportamento de frameworks e bibliotecas populares'

- - -

### O objetivo: explorar o padrão MVC


1. Isolamos **Model**: 

A Negociação é criada com um construtor que usa o método `freeze` no objeto. Os *getters** foram aplicados já implementando a convenção, para Javascript, de *underline* sinalizando atributos "privados".

A lista de Negociações, como array que agrupa as mesmas, foi criada como um modelo, também. Assim como a classe `Mensagem`, que foi criada de forma *genérica* em padrão passível de ser repetido e adaptado.


2. Nossas **Views**:

Temos uma classe abstrata `View`. Nela concentramos o construtor em comum, e o método `update()` que consulta alterações para exibição. O método `template()`, aqui, recebe apenas o tratamento de exceção, que indica que ele deve ser sobrescrito.

A classe `MensagemView` extende a classe abstrata, para exibição das mensagens informando ao usuário, por exemplo, da inserção de nova negociação. O retorno deste método é dirigido ao DOM em uma tag `<p>` com somente esta finalidade.

Da mesma forma, a classe `NegociacoesView` extende a mesma classe `View` para exibição, no DOM, das negociações inseridas via input. 

Aqui, o método `update()` ganha complexidade. Uma template string é aberta com o código *HTML* da tabela inteira a ser exibida. 

No `<tbody>`, o `model` - o array montado a partir dos input - é passado como parâmetro para uma função `map()`, que percorre estes elementos. Aqui usamos o `DateHelper` (descrito a seguir) para tratar nosso Objeto Date, e montamos as `<td>` que serão exibidas nos campos da tabela. 

Este array é agrupado em string por um `join('')`. E assim inserimos no documento HTML nossa template string já como uma tabela montada de forma dinâmica.


3. E nosso **Controller**:

A classe `NegociacaoController`, é o core da aplicação, que comunica os modelos criados com a interação do usuário, e as views. Seu construtor faz a manipulação do DOM. 

Usamos *binding* para abreviar o `querySelector` em uma variável simples (`$`). Os atributos são selecionados por `id`, do documento HTML.

Ali acontecem as instâncias de novas mensagens e negociações, no `constructor` e também no método `adiciona`. Este último está vinculado ao evento `onsubmit` do formulário.

Dois métodos privados surgem aí: `_limpaFormulário` reseta os campos, e `_criaNegociacao` resgata os dados do input para criar uma instância de `Negociacao`. 


Neste ponto entra a manipulação de data:


### Date Object can be tricky

Para melhor aproveitamento do Objeto do tipo Date, criamos o *ajudante* na classe abstrata `DateHelper`. No construtor é tratada a exceção, e os métodos são declarados como estáticos, para serem diretamente referenciados a partir do controller. 
Os métodos aqui isolam responsabilidades de conversão de valores. 
Objeto Date tem particularidades de formato que devem ser ajustadas para nossa exibição. O valor `mês` vai de `0` a `11`, o que, para o usuário não é interessante.

O método `dataParaTexto(data)` recebe uma data, é criado como uma boa prática, é mais simples. Composto por métodos do próprio objeto Date, corrige o mês, e monta uma data separada por `/`, montada com template string. É usado na montagem da `<td>` para exibir um valor que faça sentido para o usuário.

O método `textoParaData(texto)` recebe a string típica do objeto Date, entra na validação do input. 

Usamos `split()` para fazer da string um array, separando elementos por `-`. 

Também um `map()` para percorrer estes itens e fazer a subtração devida no elemento de índice 1, o *mês*, que deve ser ajustado. Este processo é executado se não entramos na condicional que, usando `RegEx`, valida o que foi inserido no campo de data.




>   Esta foi a primeira fase do processo de introdução ao Modelo MVC. A seguir atualizarei o projeto e este documento com o próximo treinamento, onde o padrão será estudado com maior profundidade.
