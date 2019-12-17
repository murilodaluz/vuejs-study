# Vuejs CLI - [(Doc)](https://cli.vuejs.org/guide/installation.html)
* Uma interface de linha de comando simples para auxiliar na criação de projetos Vue.js
## Instalação 
* Para realizar a instalação precisar ter na maquina o Node JS com o NPM ou o Yarn
    - Comandos
    ```sh
    npm install -g @vue/cli
    # OR
    yarn global add @vue/cli
    ```
    - Para verificar se a instalação ocorreu corretamente
    ```sh
    vue --version
    ```

## vue create
* `vue create` é o comando para iniciar a criação de um novo projeto
* Possibilita que seja criado um projeto por um *preset* já existente ou criar com uma configuração customizada
* A configuração customizada pode ser salva para tona-la um novo *preset*
    * Após o projeto criado pode ser adicionados novos plugins com o comando `vue add nomedoplugin`
## Estrutura básica padrão
* Estrutura de pastas 
    - `node_modules` - Dependências node instalados
    - `public` 
        - `index.html` - Onde os arquivos de build serão injetados
    - `src`
        - `assets`
        - `components`
        - `App.vue` - Arquivos .vue são arquivos de componentes
        - `main.js` - Porta de entrada da aplicação a onde esta sendo criado a instancia do VueJS
    - `.gitignore` - Arquivo com os arquivos e pastas que serão ignorados pelo Git
    - `babel.config.js` - Configuração do babel responsável pelo transpile normalmente do ES6 para o ES5
    - `package.json` - Arquivo com as configurações das dependências do node
## Componentes [(Doc)](https://br.vuejs.org/v2/guide/components.html)
* Componentes são estrutura isoladas de código, possibilitando a reutilização de uma implementação
* Ex: 
    ```html
    <template>
        <contador></contador>
        <contador></contador>
        <contador></contador>
    </template>
    <script>
        // Registro de forma global
        Vue.component('contador', {
            template: '
                <div>
                    <span>{{ contador }}</span> 
                    <button @click="contador++" >+</button>
                    <button @click="contador--" >-</button>
                </div>
            ',
            data: (){
                return  {
                    contador: 0
                }
            }
        })
        new Vue({
            el: "#app"
        })
    </script>
    ```
* Ex registro do componente local na instancia: 
    ```html
    <template>
        <contador></contador>
        <contador></contador>
        <contador></contador>
    </template>
    <script>
        const contador = {
            template: '
                <div>
                    <span>{{ contador }}</span> 
                    <button @click="contador++" >+</button>
                    <button @click="contador--" >-</button>
                </div>
            ',
            data: (){
                return  {
                    contador: 0
                }
            }
        }
        new Vue({
            el: "#app",
            components : {
                contador: contador
            }
        })
    </script>
    ``` 
    ### Passo a passo de criação de um componente manualmente 
    * Criando o arquivo do componente 
        ```html
        // file: Contador.vue
        <template>
            <div class="contador">
                <span>{{ contador }}</span>
                <button @click="adicionar()">+</button>
                <button @click="subtrair()" >-</button>
            </div>
        </template>
        <script>
            export default {
                data() {
                    return {
                        contador: 0
                    }
                },
                methods: {
                    adicionar() {
                        this.contador++;
                    },
                    subtrair() {
                        this.contador--;
                    }
                }
            }
        </script>
        <style>
            .contador span {
                border-bottom: 1px solid #CCC;
                height: 30px;
                padding: 5px 25px;
            }

            .contador button {
                height: 20px;
                width: 30px;
                border-radius: 15px;
                background-color: coral;
                color: #fff;
            }
        </style>
        ```
    * Importando globalmente o component na instancia do Vue.js no arquivo `main.js`
        ```javascript
        // file: main.js
        [...]
        import Contador from './Contador.vue'

        [...]
        Vue.component('app-contador', Contador);

        new Vue({
            render: h=>h(app)
        }).$mount('#app')
        ```
        * Usando o componente
        ```html
        // Nesse caso o file: App.vue
        <template>
            <app-contador></app-contador>
            <app-contador></app-contador>
            <app-contador></app-contador>
        </template>

        <script>
        </script>

        <style>
            #app {
                font-family: 'Avenir', Helvetica, Arial, sans-serif;
                -webkit-font-smoothing: antialiased;
                -moz-osx-font-smoothing: grayscale;
                text-align: center;
                color: #2c3e50;
                margin-top: 60px;
            }
        </style>
        ```
    * Caso seja interessante registar o componente de forma local estando disponível apenas dentro do outro componente
    * Não existiria o registro global no `main.js`
    * Exemplo registrando o mesmo componente de forma local
        ```html
        // Nesse caso o file: App.vue
        <template>
            <app-contador></app-contador>
            <app-contador></app-contador>
            <app-contador></app-contador>
        </template>

        <script>
            import  Contador from './Contador.vue'
            export default {
                components: {
                    appContador: Contador
                }
           }
        </script>

        <style>
            #app {
                font-family: 'Avenir', Helvetica, Arial, sans-serif;
                -webkit-font-smoothing: antialiased;
                -moz-osx-font-smoothing: grayscale;
                text-align: center;
                color: #2c3e50;
                margin-top: 60px;
            }
        </style>
        ```
    ## Style
    * estilos criados em componentes podem acabar influenciando elementos externos a ele
    * Para que isso não ocorra se utiliza a marcação `scoped` no style
        - Ex: 
        ```css
        [...]

        <style scoped>        
            span {
                border-bottom: 1px solid #CCC;
                height: 30px;
                padding: 5px 25px;
            }

            button {
                height: 20px;
                width: 30px;
                border-radius: 15px;
                background-color: coral;
                color: #fff;
            }
        </style >
        ```
    * Quando marcado como `scoped`
## Imports
* Caminho relativo quando se coloca `./path/subpath`
* Caminho absoluto `@/path/subpath/` - Previne que tenho que voltar níveis das pasta para conseguir importar o elemento.
    - Ex: `../../path/subpath` vs `@/path/subpath`

## Comunicação entre componentes

### Props [(Doc)](https://br.vuejs.org/v2/guide/components.html#Passando-Dados-aos-Filhos-com-Props)
* Uma forma de passar uma informação para um componente filho é utilizando a propriedades
* Defini-se no filho quais propriedades ele aceita e com isso podem ser interpolados os valores vindo do pai
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- Aqui sera impresso a informação que ira vir do elemento pai através da propriedade 'nome' -->
                <p>{{ nome }}</p>
            </div>
        </template>
        <script>
            export default {
                props: [ 'nome' ]
            }
        </script>
        ```
        ```html
        // componente pai
        <template>
            <div class="componente">
                <!-- Para que a propriedade 'nome' resolva o elemento data 'nome' dever ser feito o v-bind ou da forma reduzida só :  -->
                <componente-filho :nome="nome" ></componente-filho>
            </div>
        </template>
        <script>
            import ComponenteFilho from '@components/componente-filho.vue'
            export default {
                data() {
                    return {
                        nome: 'Pedro'
                    }
                }
                components: {
                    componenteFilho: ComponenteFilho
                }
            }
        </script>
        ```
* dentro do elemento filho o acesso a propriedade se da através do `this.nomedapropriedade` 
* Não pode haver uma `props` e um `data` com o mesmo nome
### Props validation [(Doc)](https://br.vuejs.org/v2/guide/components-props.html#Validacao-de-Propriedades)
* Vue também possibilita fazer algumas checagens das propriedade, como tipo, obrigatoriedade, se tem um valor default...
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- Aqui sera impresso a informação que ira vir do elemento pai através da propriedade 'nome' -->
                <p>{{ nome }}</p>
            </div>
        </template>
        <script>
            export default {
                //props: [ 'nome' ]
                //props: { nome: String } // Ira gerar um warning caso o valor passado para a propriedade não seja uma string
                props: {
                    nome: {
                        type: String,
                        // required: true,
                        default: 'Xablau'
                    }
                }
            }
        </script>
        ```
## Comunicação entre componente pai e filho
* Pode ocorrer através de um `$emit` vindo do filho que ira ser interpretado pelo pai [(Doc)](https://br.vuejs.org/v2/guide/components.html#Emitindo-um-Valor-com-um-Evento)
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- Aqui sera impresso a informação que ira vir do elemento pai através da propriedade 'nome' -->
                <p>{{ nome }}</p>
                <button @click="voltaNome()">Reinicia nome</button>
            </div>
        </template>
        <script>
            export default {
                props: [ 'nome' ],
                methods: {
                    voltaNome(){
                        this.nome = 'Pedro';
                        // this.$emit('nomeMudou', { novoNome: this.nome, antigoNome: 'Ana' ); // Só pra exemplificar que pode ser um objeto o retorno
                        this.$emit('nomeMudou', this.nome); 
                    }
                }
            }
        </script>
        ```
        ```html
        // componente pai
        <template>
            <div class="componente">
                <p>{{ nome }}</p>                                     
                <button @click="alteraNome()" > Alterar nome </button>
                <!-- Para que a propriedade 'nome' resolva o elemento data 'nome' dever ser feito o v-bind ou da forma reduzida só :  -->
                <!-- @nomeMudou é o evento criado no filho, como em um evento normal ele esta sendo ouvido e através do $event recebe o que foi passado lá no evento -->
                <componente-filho :nome="nome" @nomeMudou="nome = $event"></componente-filho>
            </div>
        </template>
        <script>
            import ComponenteFilho from '@components/componente-filho.vue'
            export default {
                data() {
                    return {
                        nome: 'Pedro'
                    }
                },
                methods:{
                    alteraNome() {
                        this.nome = 'Ana';
                    }
                }
                components: {
                    componenteFilho: ComponenteFilho
                }
            }
        </script>
        ```
* Pode ocorrer através de uma função callback passada pelo pai para o filho em uma propriedade.
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- Aqui sera impresso a informação que ira vir do elemento pai através da propriedade 'nome' -->
                <p>{{ nome }}</p>
                <!-- Botão chama pelo click a callback que veio do pai -->
                <button @click="reiniciarNome()">Reinicia nome (callback)</button>
            </div>
        </template>
        <script>
            export default {
                props: {
                    nome: String,
                    reiniciarNome: Function
                }
            }
        </script>
        ```
        ```html
        // componente pai
        <template>
            <div class="componente">
                <p>{{ nome }}</p>                                     
                <button @click="alteraNome()" > Alterar nome </button>
                <!-- Para que a propriedade 'nome' resolva o elemento data 'nome' dever ser feito o v-bind ou da forma reduzida só :  -->
                <!-- passando uma função callback pelo propriedade  'reiniciarNome' -->
                <componente-filho :nome="nome" :reiniciarNome="voltaNome"></componente-filho>
            </div>
        </template>
        <script>
            import ComponenteFilho from '@components/componente-filho.vue'
            export default {
                data() {
                    return {
                        nome: 'Pedro'
                    }
                },
                methods:{
                    alteraNome() {
                        this.nome = 'Ana';
                    },
                    voltaNome(){
                        this.nome = 'Pedro';
                    }
                }
                components: {
                    componenteFilho: ComponenteFilho
                }
            }
        </script>
        ```
#### Event bus 
* caso tenha muito niveis para propagar a mensagem pode ser usado essa conceito se a aplicação foi simples
## Slot [(Doc)](https://br.vuejs.org/v2/guide/components.html#Distribuicao-de-Conteudo-com-Slots)
* Para passar informações dentro do corpo da tag do componente utiliza-se a tag `slot` no componente filho.
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- slot proporciona que todos informação/html colocada dentro da corpo da declaração do componente sera renderizada aqui -->
                <slot></slot> 
            </div>
        </template>
        <script>
            export default {
            }
        </script>
        ```
        ```html
        // componente pai
        <template>
            <div class="componente">
                <componente-filho>
                    <h1> Esse HTML sera incorporado pelo "componente-filho"</h1>
                    <p> Por ter a tag Slot </p>
                </componente-filho>
            </div>
        </template>
        <script>
            import ComponenteFilho from '@components/componente-filho.vue'
            export default {
            }
        </script>
        ```
* para ter mais de um slot no componente, pode-se utilizar com a tag name, para identificar cada um
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- slot proporciona que todos informação/html colocada dentro da corpo da declaração do componente sera renderizada aqui -->
                <slot name="titulo" ></slot> 
                <slot name="texto" ></slot>
            </div>
        </template>
        <script>
            export default {
            }
        </script>
        ```
        ```html
        // componente pai
        <template>
            <div class="componente">
                <componente-filho>
                    <!-- o atributo html `slot` demonstra a qual slot do componente a tag ira fazer parte -->
                    <h1 slot="titulo"> Esse HTML sera incorporado pelo "componente-filho"</h1>
                    <p slot="texto"> Por ter a tag Slot </p>
                </componente-filho>
            </div>
        </template>
        <script>
            import ComponenteFilho from '@components/componente-filho.vue'
            export default {
            }
        </script>
        ```
* Pode ser trabalhado com slot nomeado e não nomeado no mesmo componente
    * Tudo que não foi associado a uma slot nomeado sera direcionado para o slot padrão
    - Ex: 
        ```html
        // componente filho
        <template>
            <div class="componente">
                <!-- slot proporciona que todos informação/html colocada dentro da corpo da declaração do componente sera renderizada aqui -->
                <slot name="titulo" ></slot> 
                <slot></slot>
                <slot name="texto" ></slot>
            </div>
        </template>
        <script>
            export default {
            }
        </script>
        ```
        ```html
        // componente pai
        <template>
            <div class="componente">
                <componente-filho>
                    <!-- o atributo html `slot` demonstra a qual slot do componente a tag ira fazer parte -->
                    <h1 slot="titulo"> Esse HTML sera incorporado pelo "componente-filho" no slot nomeado como titulo</h1>
                    <p> Esse vai para no slot padrão </p>
                    <p slot="texto"> Esse vai para o slot nomeado com texto</p>
                </componente-filho>
            </div>
        </template>
        <script>
            import ComponenteFilho from '@components/componente-filho.vue'
            export default {
            }
        </script>
        ```
## Tag component do Vuejs [(Doc)](https://br.vuejs.org/v2/guide/components.html#Componentes-Dinamicos)
* Trabalha o conceito do componente dinâmico `<component :is="'NomeDoComponent'"></component>`
* Elementos dinâmicos quando trocarem o componente ele ira criar e destruir o component a cada interação
    * para mante-lo deve ser envolto na tela `<keep-alive></keep-alive>`
    * as funções que definem o life-cycle do keep alive são `activated` e `deactivated`
