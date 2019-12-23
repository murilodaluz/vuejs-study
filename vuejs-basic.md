## Vue
* Vue é um framework para desenvolvimento do front-end 
* Suas principais características 
    - Virtual Dom - É uma representação leve em memoria da arvore do DOM html original.
    - Componentes - Usado para criar elementos reutilizáveis em uma aplicação VueJs
    - Templates - Prove templates baseados em HTML que lincados com o DOM através da instancia do Vue data
    - Routing - Navegação por paginas é possível pelo vue-router
    - Leve - VueJs é uma biblioteca leve comparada com os outros frameworks
* Ex:
    ```html
    <template>
        <div id="app" v-on:input="alteraTitulo">
            <p>{{ titulo }}</p>
        </div>
    </template>
    <script>
    new Vue({
        el: '#app',
        data: {
            titulo: 'Usando as parada'
        },
        methods: {
            alterarTitulo(event){
                this.titulo = event.target.value;
            }
        }
    })
    </script>
    ```
## Instalação
* [Doc](https://cli.vuejs.org/guide/installation.html)

## Estrutura básica - [(Doc)](https://vuejs.org/v2/guide/index.html#Declarative-Rendering)
* Template - Html que sera renderizar pelo vue
* Script - Definições de comportamento e dados do componente
    * new Vue({}) instancia do Vuejs
        * `el` - Recebera a referencia referente a tag html que sera controlada. 
        * `data` - Atributos com os dados que serão trabalhados dentro da estrutura do vuejs
        * `methods` - Métodos e funções que iram ser associadas a estrutura do Vuejs
            * Para acessar os dados e métodos do vue dentro de um outro método utilizada-se `this.nomedoAtributo`
    * Todas as informações criadas dentro da instancia do Vue são criadas como reativas
    - Ex:
    ```html
    <div id="app">
        <p>{{ nome }}</p>
        <p>{{ getNome() }}</p>
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            nome: 'Murilo',
            segundoNome: ' Da Luz'
        },
        methods: {
            getNome() {
                return this.nome;
            },
            getFullName(){
                return this.getNome() + this.segundoNome;
            }
        }
    })
    </script>
    ```

## Binding - [(Ligação ;)](https://br.vuejs.org/v2/guide/class-and-style.html)
* Para poder utilizar os dados resolvido nos atributos do html deve ser feito o binding da informação com a diretiva. Para isso utiliza-se o `v-bind:nomeDaDiretiva='dado'` ou na sintaxe reduzida `:nomeDaDiretiva='dado'`
* Ex:
    ```html
    <div id="app">
        <a v-bind:href="link"></a>
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            link: "http://google.com"
        }
    })
    </script>
    ```
* Todo os dados contidos no objeto `data` possuem a ligação com as atualizações da variável, sendo assim sempre que uma informação for atualizada na variável ira refletir onde ela esta sendo interpolada.
- Ex:
    ```html
    <div id="app">
        <p>{{ nome }}</p> // Ira imprimir primeiro 'Murilo' depois da chamada do changeFirstName ira atualizar para 'Outro nome'
        <p>{{ changeFirstName() }}</p> // Ira imprimir 'Outro nome'
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            nome: 'Murilo',
            segundoNome: ' Da Luz'
        },
        methods: {
            getNome() {
                return this.nome;
            },
            getFullName(){
                return this.getNome() + this.segundoNome;
            },
            changeFirstName(){
                this.nome = 'Outro Nome';
                return this.nome;
            }
        }
    })
    </script>
    ```
* Para impedir essa alteração no que já foi impresso na tela pode ser utilizado a diretiva `v-once`
- Ex:
    ```html
    <div id="app">
        <p v-once>{{ nome }}</p>// Ira imprimir primeiro 'Murilo' depois da chamada do changeFirstName não ira alterar
        <p>{{ changeFirstName() }}</p> // Ira imprimir 'Outro nome'
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            nome: 'Murilo',
            segundoNome: ' Da Luz'
        },
        methods: {
            getNome() {
                return this.nome;
            },
            getFullName(){
                return this.getNome() + this.segundoNome;
            },
            changeFirstName(){
                this.nome = 'Outro Nome';
                return this.nome;
            }
        }
    })
    </script>
    ```
## Eventos [(Doc)](https://br.vuejs.org/v2/guide/events.html)
* Para poder escutar os eventos no html dentro do Vue utiliza-se o atributo `v-on:oevento="callback"` ou na sintaxe reduzida `@oevento="callback"`
* Ex: 
    ```html
    <div id="app">
        <p>{{ contador }}</p>
        <button v-on:click="somarMaisUm" >+1</button>
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            contador: 0
        },
        methods: {
            somarMaisUm(){
                this.contador++;
            }
        }
    })
    </script>
    ```
* Todas as funções sem parâmetro que são atribuídas para escutar um evento, receber por padrão um parâmetro com o evento. 
* Ex: 
    ```html
    <div id="app">
        <p>{{ contador }}</p>
        <button v-on:click="somarMaisUm" >+1</button>
        <p v-on:mousemove="atualizarXY">Posição do Mouse ({{x}},{{y}}) </p>
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            contador: 0,
            x: 0,
            y: 0
        },
        methods: {
            somarMaisUm(){
                this.contador++;
            },
            atualizarXY(event){
                this.x = event.clientX;
                this.y = event.clientY;
            }
        }
    })
    </script>
    ```
* Param quando é passado por parâmetro alguma informação, pode ser acessado o evento com a palavra reservada `$event`

* Ex: 
    ```html
    <div id="app">
        <p>{{ contador }}</p>
        <button v-on:click="somarMaisUm(5, $event)" >+5</button>
        <p v-on:mousemove="atualizarXY">Posição do Mouse ({{x}},{{y}}) </p>
    </div>
    <script>
    new Vue({
        el: '#app',
        data: {
            contador: 0,
            x: 0,
            y: 0
        },
        methods: {
            somarMaisUm(step, event){
                console.log(event);
                this.contador+=step;
            },
            atualizarXY(event){
                this.x = event.clientX;
                this.y = event.clientY;
            }
        }
    })
    </script>
    ```
### Modificadores de evento -  [(Doc)](https://vuejs.org/v2/guide/events.html#Event-Modifiers)
* Em muitos casos o comportamento padrão do evento deve ser sobrescrito, com isso utilizando os modificadores de evento, para trabalhar o comportamento dele.
- Ex:
    ```html
    <!-- From: vuejs.org  -->
    <!-- the click event's propagation will be stopped -->
    <a v-on:click.stop="doThis"></a>

    <!-- the submit event will no longer reload the page -->
    <form v-on:submit.prevent="onSubmit"></form>

    <!-- modifiers can be chained -->
    <a v-on:click.stop.prevent="doThat"></a>

    <!-- just the modifier -->
    <form v-on:submit.prevent></form>

    <!-- use capture mode when adding the event listener -->
    <!-- i.e. an event targeting an inner element is handled here before being handled by that element -->
    <div v-on:click.capture="doThis">...</div>

    <!-- only trigger handler if event.target is the element itself -->
    <!-- i.e. not from a child element -->
    <div v-on:click.self="doThat">...</div>
    ```
### Modificadores de teclas [(Doc)](https://vuejs.org/v2/guide/events.html#Key-Modifiers)

* Ao escutar o evento de pressionar teclas, existindo a necessidade de verificar qual tecla foi pressionada existe no Vue já um modificador de acesso do evento especifico para identificar teclas.
- Ex:
    ```html
    <!-- only call `vm.submit()` when the `key` is `Enter` -->
    <input v-on:keyup.enter="submit">
    ```
* Possibilitando ainda utilizar com as teclas de [Ctrl, alt...](https://vuejs.org/v2/guide/events.html#System-Modifier-Keys)
- Ex:
    ```html
    <!-- Alt + C -->
    <input @keyup.alt.67="clear">

    <!-- Ctrl + Click -->
    <div @click.ctrl="doSomething">Do something</div>
    ```
## Interpolação
* No Vue a interpolação de da pelo sintaxe Mustache (chaves duplas). 
* Dentro dela é possível realizar os cálculos simples, condições ternarias...
    * `{{ (variavel * 2 ) + 10 }}` ou `{{ (variavel > 10)? 'Maior que 10' : 'Menor que 10' }}`
    ### Computed [(Doc)](https://br.vuejs.org/v2/guide/computed.html#Dados-Computados)
    * Para evitar uso desnecessária de recurso, usa-se os dados computados que irão ser cacheados de acordo com suas \
    dependências reativas. Um dado computado somente sera reavaliado quando alguma de suas dependências forem alteradas \
    se não era retornar o valor em cache sem precisar reprocessar a informação.
    * Ex:
    ```html
    <div id="app">
        <button v-on:click="aumentar">Aumentar</button>
        <button v-on:click="aumentar2">Aumentar 2</button>
        <button v-on:click="diminuir">Diminuir</button>
        <p>Contador: {{ contador }} | {{ contador2 }} </p>
        <p>Resultado: {{ resultado }}</p>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                aumentar: 0,
                aumentar2: 0
            },
            computed: {
                resultado() {
                    // **** Essa função só sera chamada quando a função 'aumentar' for chamada, pois é ela que altera a propriedade 'contador' ****
                    console.log("Método chamado...");
                    return this.contador >= 5 ? 'Maior ou igual a 5': 'Menor que 5';
                }
            },
            method: {
                aumentar() {
                    this.aumentar++;
                },
                aumentar2() {
                    this.aumentar2++;
                },
                // ***** Se fosse usado essa função para interpolar o valor no resultado, no html deveria ser chamado como um função 'resultado()' e a cada renderização do DOM ele seria chamado tendo ou não atualizado o valor da propriedade 'contador' *****          
                //resultado() {
                //console.log("Método chamado...");
                //return this.contador >= 5 ? 'Maior ou igual a 5': 'Menor que 5';
                //}
            }
        })
    </script>
    ```
    ### Watch [(Doc)](https://br.vuejs.org/v2/guide/computed.html#Observadores)
    * O `watch` serve para escutar a alteração de uma propriedade e com isso executar ação ---- watch deep
    * Para usar o `watch`, adiciona-se um novo objeto com as funções que callback contendo o mesmo nome da propriedade presente no `data`
    * Uma função do `watch` sempre recebe no primeiro parâmetro o novo valor e no segundo parâmetro o valor antigo
    * Ex:
    ```html
    <div id="app">
        <button v-on:click="aumentar">Aumentar</button>
        <button v-on:click="aumentar2">Aumentar 2</button>
        <button v-on:click="diminuir">Diminuir</button>
        <p>Contador: {{ contador }} | {{ contador2 }} </p>
        <p>Resultado: {{ resultado }}</p>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                contador:0,
                resultado: '',
                aumentar: 0,
                aumentar2: 0
            },
            computed: {
                resultado() {
                    // **** Essa função só sera chamada quando a função 'aumentar' for chamada, pois é ela que altera a propriedade 'contador' ****
                    console.log("Método chamado...");
                    return this.contador >= 5 ? 'Maior ou igual a 5': 'Menor que 5';
                }
            },
            watch: {
                contador(novo, antigo):{
                    // O uso da ArrowFunction favorece pois o 'this' permanecesse léxico, sendo do mesmo escopo.
                    // Se utiliza-se com uma 'function' normal iria gerar um 'this' novo em outro escopo (this não léxico), tendo então que gerar uma variável que receba o 'this' e depois ser utilizada dentro da função.
                    setTimeout(() => {
                        this.contador = 0
                    }, 1000);                    
                }
            }
            method: {
                aumentar() {
                    this.aumentar++;
                },
                aumentar2() {
                    this.aumentar2++;
                },
                // ***** Se fosse usado essa função para interpolar o valor no resultado, no html deveria ser chamado como um função 'resultado()' e a cada renderização do DOM ele seria chamado tendo ou não atualizado o valor da propriedade 'contador' *****          
                //resultado() {
                //console.log("Método chamado...");
                //return this.contador >= 5 ? 'Maior ou igual a 5': 'Menor que 5';
                //}
            }
        })
    </script>
    ```
## Style
* Para complementar o uso de estilos o Vue disponibiliza alguns recursos para trabalhar com o CSS
* Caso queira adicionar uma Classe de forma dinâmica ao componente pode ser utilizado o `v-bind:class="{'nomedaclasse': condicaoBooleana }"`
    * Caso seja uma expressão muito complexa para definir a classe que ira ser implementada, pode ser utilizado um função computada para deixar o template mais enxuto.
    - Ex:
        ```html
        <style>
            #app {
                display: flex;
                justify-content: space-around;
            }

            .caixa {
                width: 100px;
                height: 100px;
                background-color: gray;
            }

            .cor1 {
                background-color: red;
            }

            .cor2 {
                background-color: blue;
            }
        </style>
        <div id="app">
            <div class="caixas" >
                <div class="caixa" :class="{ 'cor1': aplicaCor1 }" ></div>
                <div class="caixa" :class="estilo1" @click="aplicaCor1 = != aplicaCor1"></div>
                <div class="caixa" :class="[classeCSSCor1]" ></div>
            </div>
        </div>
        <script>
            new Vue({
                el: '#app',
                data: {
                    aplicaCor1: false,
                    classeCSSCor1: 'cor1'
                },
                computed: {
                    estilo1(){
                        return {
                            cor1: this.aplicaCor1,
                            cor2: !this.aplicaCor1
                       }
                    }
                }
            })
        </script>
        ```
        * Para utilizar de forma direta a implementação do style sem utilizar uma classe CSS utiliza-se o atributo  `:style="valores"`
        * Ex: `:style="{ 'background-color': red }"`, `:style="{ backgroundColor: red }`...
- Exemplo para uma barra de progresso
```html
<style>
	.barra-progresso {
		height: 30px;
		width: 500px;
		border: 1px solid #000;
	}

	.progresso {
		background-color: red;
		height: 100%;
	}
</style>

<div id="app">
	<div>
		<button @click="iniciarProgresso">Iniciar</button>
		<div class="barra-progresso">
			<div class="progresso" :style="{width}"></div>
		</div>
	</div>
</div>
<script>
	new Vue({
		el: '#app',
		data: {
			width: '0'
		},
		methods: {
			iniciarProgresso() {
				let valor = 0
				const temporizador = setInterval(() => {
					valor += 5
					this.width = `${valor}%`
					if(valor == 100) clearInterval(temporizador)
				}, 500)
			}
		}
	})
</script>
```
## Condicionais [(Doc)](https://br.vuejs.org/v2/guide/conditional.html#v-if)
* Quando precisa ser definido a partir de uma condição o que deve ser apresentado na tela, o Vue traz algumas diretivas para tratar as condições.
* `v-if`, `v-if-else` e `v-else`, para funcionar o `v-if-else` e o `v-else` sempre tem que vir no componente seguinte a o `v-if`
* `v-if` remove e coloca o elemento na DOM dependendo da condição.
- Ex: 
    ```html
    <div id="app">
        <p v-if="logado"> Usuário logado: {{nome}} </p>
        <p v-else-if="anonimo"> Navegando como anonimo </p>
        <p v-if="logado"> Nenhum usuário logado </p>
        <button @click="logado = !logado" > {{ (logado)? 'Sair' : 'Entrar' }} </button>
        <input type="checkbox" v-model="anonimo" >
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                nome: 'Maria',
                logado: false,
                anonimo: false
            }
        })
    </script>
    ```
* Para trabalhar com condições sem alterar a DOM a cada interação pode ser utilizado o `v-show`
```html
<div id="app">
    <div v-show="mostra"></div>
    <button @click="mostra = !mostra"></button>
</div>
<script>
new Vue({
    el: '#app',
    data:{
        mostra: false
    }
})
</script>
```
## Laços de repetição [(Doc)](https://br.vuejs.org/v2/guide/list.html#Array-em-Elementos-com-v-for)
* Para trabalhar com listagem, podendo renderizar os valores de uma lista, existe a diretiva `v-for`
- Ex:
    ```html
    <div>
        <ul>
            <li v-for="cor in cores"> {{ cor }} </li>
        </ul>
        <ul>
            <!-- para ter acesso ao index da lista -->
            <li v-for="(cor, index) in cores"> {{index + ")"}} {{ cor }} </li>
        </ul>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                cores: [ 'vermelho', 'amarelo', 'preto', 'rosa', 'azul', 'verde' ]

            }
        })
    </script>
    ```
* Como no javascript que o forEach pode ser utilizado para percorrer os elementos de um objecto o `v-for` também permite.
* Para facilitar o trabalho do VueJs para quando houver alterações na lista e ele conseguir renderizar de forma satisfatória utiliza-se a `:key` ou dica em que o VueJs era utilizada para rastrear as alterações de ordenação e mudanças na lista. [(Doc)](https://br.vuejs.org/v2/guide/list.html#Maintaining-State)
- Ex:
    ```html
    <div>
        <ul>
            <li v-for="pessoa in pessoas" :key="pessoa.id"> 
                <div v-for="(valor, chave, index) in pessoa" >
                    {{chave}} = {{valor}}
                </div>
            </li>
        </ul>
    </div>
    <script>
        new Vue({
            el: '#app',
            data: {
                pessoas: [
                    { id: 1, nome: 'Ana', idade: 29, peso: 45 },
                    { id: 2, nome: 'Xablau', idade: 30, peso: 90 }
                ]
            }
        })
    </script>
    ```
* Para iterar um range de valores independendo de um tamanho de array com o `v-for` pode ser passado um valor inteiro.
- Ex:
    ```html
    <div v-for="n in 10">
        {{ n }}
    </div>
    ```
## Dom virtual
* VueJs trabalha como conceito de uma DOM virtual, ou seja uma cópia do estado da DOM no ambiente do javascript, com isso possibilitando uma melhor performance na renderização dos componente.
* Possibilita que o Vue analise dentro da DOM virtual, quais os elementos que devem ser renderizado de uma forma precisa
    ### Ciclo de vida da intancia [(Doc)](https://br.vuejs.org/v2/guide/instance.html#Ciclo-de-Vida-da-Instancia)
    ![Life cycle - Vuejs.org](./images/lifecycle.png)

    * Para interação com cada etapa do ciclo de vida da instancia existem métodos para interagir com os estados.
    ```html
    <div id="app">
        <h1>{{ titulo }}</h1>
        <button @click="titulo += '#'"> Alterar titulo </button> 
        <button @click="$destroy()"> Destruir </button>
    </div>
    <script>
        new Vue({
            el: "#app",
            data: {
                titulo: 'VueJs'
            },
            beforeCreate(){
                console.log('Antes de criar');
            },
            created(){
                console.log('criado');
            },
            beforeMount(){
                console.log('Antes de montar (DOM)');
            },
            mounted(){
                console.log('Montado na DOM');
            },
            beforeUpdate(){
                console.log('Antes de atualizar');
            },
            updated(){
                console.log('Atualizado');
            },
            beforeDestroy(){
                console.log('Antes de destruir');
            },
            destroyed(){
                console.log('Destruído');
            }

        })
    </script> 
    ```
