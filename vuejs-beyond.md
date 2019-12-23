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
    - Ex: 
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
    - Ex registro do componente local na instancia: 
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
* caso tenha muito níveis para propagar a mensagem pode ser usado essa conceito se a aplicação foi simples
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
## Forms - Manipulando dados formulário [(Doc)](https://br.vuejs.org/v2/guide/forms.html)
* Para realizar o binding das informações com um input utiliza-se o `v-model`
    - Ex: `<input v-model="usuario.email" ></input>`
* Existem tres modificadores de input com o `v-model`:
    * `lazy` : Ira realizar o bind somente quando o input perder o foco 
    * `trim` : Removera os espaços nas extremidades
    * `number` : Converte os valores que são numero para o tipo number (por padrão o input no bind acaba convertendo para string.)
    - Ex:
        ```html
        <template>
            <div class="content">
                <!-- O 'trim' pode ser encadeado com o lazy ou sozinho -->
                <input type="text" v-mode.lazy.trim="usuario.email" />
                <!-- Faz com que o valor passado no bind não seja convertido para uma string e sim para um numero -->
                <input type="number" v-mode.number="usuario.idade" />
            </div>
        </template>
        <script>
            export default {
                data() {
                    return {
                        usuario: {
                            email: '',
                            idade: 27
                        }
                    }
                }
            }
        </script>
        ```
* Quando o input é do tipo `checkbox` o VueJs consegue associar os valores a um array \
basta o bind ser feito em cada elemento com a mesma variável array.
    - Ex:
        ```html
        <template>
            <div class="content">
                <!-- Todos as checkbox que fazem parte do mesmo grupo iram fazer o bind com o mesmo array -->
                <span><input type="checkbox" v-model="itens" value="batata"> Batata </span>
                <span><input type="checkbox" v-model="itens" value="arroz"> Arroz </span>
            </div>
        </template>
        <script>
            export default {
                data() {
                    return {
                        itens: []
                    }
                }
            }
        </script>
        ```
* Para input do tipo `radio` também se associa todos a uma mesma variável no bind
    - Ex:
        ```html
        <template>
            <div class="content">
                <!-- Todos as checkbox que fazem parte do mesmo grupo iram fazer o bind com o mesmo array -->
                <span><input type="radio" v-model="tipoProduto" value="grao"> Grão </span>
                <span><input type="radio" v-model="tipoProduto" value="fruta"> Fruta </span>
            </div>
        </template>
        <script>
            export default {
                data() {
                    return {
                        tipoProduto: 'grao' // Inicializando valor como 'grao', mas é opcional
                    }
                }
            }
        </script>
        ```
* Para `select` o bind ocorre com o v-model normal
    - Ex:
        ```html
        <template>
            <div class="content">
                <select v-model="prioridade">
                    <option v-for="prioridade in prioridades"
                        :value="prioridade.id"
                        :key="prioridade.id">
                        {{ prioridade.nome }}</option>
                </select>
            </div>
        </template>
        <script>
            export default {
                data() {
                    return {
                        prioridade: 1,
                        prioridades: [
                            { id: 1, nome: "Baixa" },
                            { id: 2, nome: "Média" },
                            { id: 3, nome: "Alta"  },
                        ]
                    }
                }
            }
        </script>
        ```
* Para realizar o bind de um componente personalizado com uma variável do pai deve ser tratado pelo `$emit`
* Sendo que um componente do tipo input no `v-model` se espera eventos do tipo input, logo devemos emitir um evento do tipo input
    - Ex:
        - `Componente`
        ```html
        <template>
            <div class="hello">
                <!-- esta escutando o evento que ele emit -->
                <input :value="value" @input="$emit('input', $event.target.value)" /> 
            </div>
        </template>

        <script>
            export default {
                name: 'HelloWorld',
                props: [ 'value' ]
            }
        </script>
        ```
        - Usando o componente
        ```html
        <template>
            <div id="app">
                <HelloWorld v-model="value"/>
                {{ value }}
            </div>
        </template>

        <script>
            import HelloWorld from './components/HelloWorld.vue'

            export default {
                name: 'app',
                data() {
                    return {
                        value: 'Welcome'
                    }
                },
                components: {
                    HelloWorld
                }
            }
        </script>
        ```
* Para a submeter um formulário utiliza-se o `prevent` no evento `click`
    - Ex:
        ```html
        <template>
            <div class="content">
                <div v-show="!salvo" >
                    <select v-model="prioridade">
                    <option v-for="prioridade in prioridades"
                            :value="prioridade.id"
                            :key="prioridade.id">
                            {{ prioridade.nome }}</option>
                    </select>
                    <!-- Utilizando o prevent e associando a função enviar -->
                    <button @click.prevent="enviar"> Salvar </button>
                </div>
                <div v-else >
                    {{ prioridade }}
                </div>
            </div>
                
        </template>
        <script>
            export default {
                data() {
                    return {
                        salvo: false,
                        prioridade: 1,
                        prioridades: [
                            { id: 1, nome: "Baixa" },
                            { id: 2, nome: "Média" },
                            { id: 3, nome: "Alta"  },
                        ]
                    }
                },
                methods:{
                    enviar() {
                        this.salvo = true;
                        alert('Salvo com sucesso!');
                    }
                }
            }
        </script>
        ```
## Diretivas [(Doc)](https://vuejs.org/v2/guide/custom-directive.html)
* Assim como `v-model` e `v-show` é possível criar diretivas personalizadas
    * As diretivas no Vue possuem os gatilhos [(hooks)](https://vuejs.org/v2/guide/custom-directive.html#Hook-Functions)  `bind`, `inserted`, `updated`, `componentUpdated` e `unbind`
    * E os parâmetros dos gatilhos [(Hook Arguments)](https://vuejs.org/v2/guide/custom-directive.html#Directive-Hook-Arguments) `el`, `binding` e `vnode`
    * Como a propriá documentação trata, o `el` é o único que pode ter seus valores alterados, o `binding` e o `vnode` deve ser considerados readonly
    - Ex diretiva: 
    ```javascript
    import Vue  from 'vue'
    Vue.directive('destaque', {
        bind (el, binding, vnode){
            // A diretiva ira receber um valor que sera atribuído com cor do fundo do elemento que ela esta associada
            el.style.backgroundColor = binding.value;
        }
    })
    ```
    ```html
    <template>
        <!-- A diretiva que sempre vai ter o prefixo 'v-' esta recebendo a string 'red' -->
        <p v-destaque="'red'"> Usando a diretiva</p>
        <!-- A diretiva esta recebendo a variável corDoFundo que ela ira interpretar e ler o conteúdo que é 'blue' -->
        <p v-destaque="corDoFundo"> Usando a diretiva</p>
    </template>
    <script>
        export default {
            data(){
                corDoFundo: 'blue'
            }
        }
    </script>
    ```
* Na anatomia de uma diretiva temos `diretiva:argumento.modificador1.modificador2.modificadorn="value"`
    * no parâmetro `binding` disponibiliza o argumento em `binding.arg`
    - Ex: 
        ```javascript
        import Vue  from 'vue'
        Vue.directive('destaque', {
            bind (el, binding, vnode){
                // Verifica o argumento que foi passado na diretiva se é igual a fundo
                if(binding.arg == 'fundo'){
                    // A diretiva ira receber um valor que sera atribuído com cor do fundo do elemento que ela esta associada
                    el.style.backgroundColor = binding.value;
                } else {
                    el.style.color = binding.value;
                }
            }
        })
        ```
        ```html
        <template>
            <!-- A diretiva que sempre vai ter o prefixo 'v-' esta recebendo a string 'red' -->
            <!-- ':fundo' é a sintaxe para passar a diretiva um argumento -->
            <p v-destaque:fundo="'red'"> Usando a diretiva</p>
            <!-- A diretiva esta recebendo a variável corDoFundo que ela ira interpretar e ler o conteúdo que é 'blue' -->
            <!-- Nesse caso não é passado nenhum argumento -->
            <p v-destaque="corDoFundo"> Usando a diretiva</p>
        </template>
        <script>
            export default {
                data(){
                    corDoFundo: 'blue'
                }
            }
        </script>
        ```
    * no parâmetro `binding` disponibiliza os modificadores em `binding.modifiers`
        * Sendo um array no `modifiers` vai estar disponível todos os modificadores associados na diretiva
    - Ex: 
        ```javascript
        import Vue  from 'vue'
        Vue.directive('destaque', {
            //  Declaração local da diretiva
            bind (el, binding, vnode){
                let atraso = 0

                // Verificando se o modificador 'atrasar' foi associado na diretiva
                if(binding.modifiers['atrasar']){
                    atraso = 3000
                }

                if(binding.modifiers['alternar']){
                    // Faz algo legal ai......
                }

                setTimeout(() => {
                    // Verifica o argumento que foi passado na diretiva se é igual a fundo
                    if(binding.arg == 'fundo'){
                        // A diretiva ira receber um valor que sera atribuído com cor do fundo do elemento que ela esta associada
                        el.style.backgroundColor = binding.value;
                    } else {
                        el.style.color = binding.value;
                    }
                }, atraso);
            }
        })
        ```
        ```html
        <template>
            <!-- A diretiva que sempre vai ter o prefixo 'v-' esta recebendo a string 'red' -->
            <!-- ':fundo' é a sintaxe para passar a diretiva um argumento -->
            <!-- Atrelado ao argumento fundo foi associado o modificador 'atrasar' -->
            <p v-destaque:fundo.atrasar="'red'"> Usando a diretiva</p>
            <!-- A diretiva esta recebendo a variável corDoFundo que ela ira interpretar e ler o conteúdo que é 'blue' -->
            <!-- Nesse caso não é passado nenhum argumento -->
            <!-- nesse caso o modificador é usado direto -->
            <!-- E também é encadeado um segundo modificador -->
            <p v-destaque.atrasar.alternar="corDoFundo"> Usando a diretiva</p>
        </template>
        <script>
            export default {
                data(){
                    corDoFundo: 'blue'
                }
            }
        </script>
        ```
* Uma diretiva pode ter seu registro também de forma local, dentro de um componente e ela só estará disponível nesse componente 
    - Ex:
        ```html
        <template>
            <!-- USANDO A DIRETIVA QUE FOI DECLARADA LOCAL NO PRÓPRIO COMPONENTE -->
            <!-- A diretiva que sempre vai ter o prefixo 'v-' esta recebendo a string 'red' -->
            <!-- ':fundo' é a sintaxe para passar a diretiva um argumento -->
            <!-- Atrelado ao argumento fundo foi associado o modificador 'atrasar' -->
            <p v-destaque:fundo.atrasar="'red'"> Usando a diretiva</p>
            <!-- A diretiva esta recebendo a variável corDoFundo que ela ira interpretar e ler o conteúdo que é 'blue' -->
            <!-- Nesse caso não é passado nenhum argumento -->
            <!-- nesse caso o modificador é usado direto -->
            <p v-destaque.atrasar="corDoFundo"> Usando a diretiva</p>
        </template>
        <script>
            export default {
                data(){
                    corDoFundo: 'blue'
                },
                directives: {
                    'destaque': {
                        //  Declaração local da diretiva
                        bind (el, binding, vnode){
                            let atraso = 0

                            // Verificando se o modificador 'atrasar' foi associado na diretiva
                            if(binding.modifiers['atrasar']){
                                atraso = 3000
                            }

                            setTimeout(() => {
                                // Verifica o argumento que foi passado na diretiva se é igual a fundo
                                if(binding.arg == 'fundo'){
                                    // A diretiva ira receber um valor que sera atribuído com cor do fundo do elemento que ela esta associada
                                    el.style.backgroundColor = binding.value;
                                } else {
                                    el.style.color = binding.value;
                                }
                            }, atraso);
                        }
                    }
                }
            }
        </script>
        ```

## Filters [(Doc)](https://br.vuejs.org/v2/guide/filters.html)
* Filtros são utilizado para atualizar no valor que esta sendo interpolado ou no `v-bind`
    - Ex:
        ```html
        <template>
            <!-- ira aplicar o filter maskCpf ao valor da variável cpf -->
            <p>{{ cpf | maskCpf }}</p>
        </template>
        <script>
            export default {
                filters: {
                    maskCpf(valor){
                        const arr = valor.split('');
                        arr.splice(3, 0, ".");
                        arr.splice(7, 0, ".");
                        arr.splice(11, 0, "-");

                        // Esse retorno substituirá o valor que esta sendo interpolado, no caso vai passar o cpf com a mascara
                        return arr.join('');
                    }
                }
                data(){
                    cpf: '01400887202'
                }
            }
        </script>
        ```
    * Um filter também pode ser registrado globalmente
    - Ex:
        ```javascript
        import Vue from 'vue'
        [...]

        Vue.filter('inverter', function(valor){
            valor.split('').reverse().join('');
        })

        [...]
        ```
        ```html
        <template>
            <!-- ira aplicar o filter maskCpf ao valor da variável cpf -->
            <!-- 'inverter' é o filter registrado globalmente -->
            <p>{{ cpf | maskCpf | inverter }}</p>
        </template>
        <script>
            export default {
                filters: {
                    maskCpf(valor){
                        const arr = valor.split('');
                        arr.splice(3, 0, ".");
                        arr.splice(7, 0, ".");
                        arr.splice(11, 0, "-");

                        // Esse retorno substituirá o valor que esta sendo interpolado, no caso vai passar o cpf com a mascara
                        return arr.join('');
                    }
                }
                data(){
                    cpf: '01400887202'
                }
            }
        </script>
        ```
## Mixins [(Doc)](https://br.vuejs.org/v2/guide/mixins.html)
* Uma solução para evitar duplicidade de código
* Ele trabalha para fazer um merge de um objeto exportado contendo `data`, `methods`, `computed`... no componente em questão
* Caso aja conflito entre nome de dados ou métodos o Vue ira dar prioridade para os declarados no componente
    - Ex:
        ```javascript
        // File: dadosDuplicados.vue
        export default {
            data() {
                return {
                    dadoduplicado: '',
                    arrayduplicado: [1, 2 ,3 ,4, 5]
                }
            },
            methods: {
                metodoDuplicado(){
                    alert("Exemplo de método duplicado");
                }
            }
        }
        ```        
        ```html
        <template>
            <!-- ira aplicar o filter maskCpf ao valor da variável cpf -->
            <p>{{ cpf | maskCpf }}</p>
            <p>{{ dadoduplicado }}</p>
            <span v-for="duplicado in arrayduplicado">
                {{ duplicado }}</span>
            <button @click="metodoDuplicado"> Chama método duplicado </button>
        </template>
        <script>
            import dadosDuplicados from './dadosDuplicados'
            export default {
                //Nesse momento com o mixins sera mesclado os dados e métodos do arquivo dadosDuplicados
                mixins: [ dadosDuplicados ]
                filters: {
                    maskCpf(valor){
                        const arr = valor.split('');
                        arr.splice(3, 0, ".");
                        arr.splice(7, 0, ".");
                        arr.splice(11, 0, "-");

                        // Esse retorno substituirá o valor que esta sendo interpolado, no caso vai passar o cpf com a mascara
                        return arr.join('');
                    }
                }
                data(){
                    cpf: '01400887202'
                }
            }
        </script>
        ```
    * mixins pode ser definido também de forma global
    - Ex:
        ```javascript
        import Vue from 'vue'
        [...]

        Vue.mixins({
            created(){
                // Sendo um método do ciclo de vida em um mixins global, sera chamado em todas os componente da aplicação inclusive na instancia do vue
                console.log('Created - mixing global');
            }
        })

        [...]
        ```
        ```html
        <template>
            <!-- ira aplicar o filter maskCpf ao valor da variável cpf -->
            <!-- 'inverter' é o filter registrado globalmente -->
            <p>{{ cpf | maskCpf | inverter }}</p>
        </template>
        <script>
            export default {
                filters: {
                    maskCpf(valor){
                        const arr = valor.split('');
                        arr.splice(3, 0, ".");
                        arr.splice(7, 0, ".");
                        arr.splice(11, 0, "-");

                        // Esse retorno substituirá o valor que esta sendo interpolado, no caso vai passar o cpf com a mascara
                        return arr.join('');
                    }
                }
                data(){
                    cpf: '01400887202'
                }
            }
        </script>
        ```
## Transição e Animações
* Para trabalhar com as transições e animações no vue, tem disponível a tag `<transition>` que ira englobar a tag que sofrera a transição
* Essas podem ser feita via css ou javascript
* No CSS ele trabalhar com classes contendo a nomenclatura padrão definida no framework
    * A nomenclatura da classe e gerada automaticamente, se na tag `<transition>` for definido um nome ele sera o prefixo se não o padrão e `v`
    * Pode ser utilizado um nomenclatura personalizada 
        - Ex: `<transition name="fade"><div> Qualquer coisa </div></transition>`  a classe fica `fade-enter`
    * As classes padrões são:
        * `*-enter` : Defini o css do primeiro frame ao começar a transição/animação
        * `*-enter-active` :  Defini o css durante a transição/animação
        * `*-enter-to` : Defini o css no final da transição/animação
        * `*-leave` : Defini o css do primeiro frame ao desfazer a transição/animação
        * `*-leave-active` : Defini o css durante a transição/animação
        * `*-leave-to` : Defini o css ao final da transição/animação de saida
    - Ex: 
        ```html
        <template>
            <button @click="exibir = !exibir"></button>
            <transition name="fade">
                <div v-if="exibir"> Div que ira participar da transição </div>
            </transition>
            <transition name="slide">
                <div v-if="exibir"> Div que ira participar da transição </div>
            </transition>
        </template>
        <script>
            data(){
                return {
                    exibir = false
                }
            }
        </script>
        <style>
            .fade-enter {
                opacity: 0;
            }

            .fade-enter-active {
                transition: opacity 2s;
            }

            .fade-enter-to {
                opacity: 1;
            }

            .fade-leave {
                opacity: 1;
            }

            .fade-leave-active {
                transition: opacity 2s;
            }

            .fade-leave-to {
                opacity: 0;
            }

            @keyframe slide-in {
                from { transform: translateY(40px); }
                to { transform: translateY(0);}
            }

            @keyframe slide-out {
                from { transform: translateY(0); }
                to { transform: translateY(40px);}
            }

            .slide-enter-active {
                animation: slide-in 2s ease;
                transition: opacity 2s;
            }

            .slide-leave-active {
                animation: slide-out 2s ease;
                transition: opacity 2s;
            }

            .slide-enter, .slide-leave-to {
                opacity: 0;
            }
        </style>
        ```
    * `type`: definira quem é o "tono" da transição/animação ou seja o tempo dele é quem vai ser contado para definir o fim
    * `appear`: para que a animação/transição ocorra no momento de carregar a tela se o componente já iniciar visível
* Essa transições/animações pode ser feitas também em javascript
    * Para trabalhar com javascript atua-se escutando os gatilhos 
    * São eles: `before-enter`, `after-enter`, `enter`, `enter-cancelled`, `before-leave`, `leave`, `after-leave` e `leave-cancelled`
    * Eles fazem parte dos ciclos de vida da tag `transition`
## Router
* `router-link`: Utilizado como gatilho para direcionar as rotas configuradas no router.js
    * `to`: Atributo utilizado para definir destino da rota
        * pode receber uma string contendo a rota ou um objeto contendo as configurações de rota
            - Ex:
                ```html
                [...]
                <router-link to="/produtos/1/editar" > 
                    Editar produto </router-link>
                <router-link :to="`/produtos/${id}/editar`" > 
                    Editar produto </router-link>
                <router-link :to="{ path:'``/produtos/${id}/editar``' params:{ id: id}, query: { complemento: true, lingua: 'pt'  }  }" > 
                    Editar produto </router-link>
                [...]
                ```
            * `path`: Propriedade para passar o rota no objeto do **to**.
            * `params`: Objeto contendo os parâmetros que serão passados para o componente.
                * Os valores da query podem ser acessados no `$route.params.nomeDoParametro`
                * Caso seja marcado a rota como **props: true** também é acessível com uma propriedade do componente
            * `query`: São os os parâmetros enviador pela query da url.
                * Os valores da query podem ser acessados no `$route.query.nomeDaVariavel`
            * `name`: Nome da rota, caso definido.

    * `tag`: por padrão o router-link gera uma tag anchor (`<a>`) no html final, para poder alterar esse comportamento utiliza-se o atributo **tag**
        - Ex:
            ```html
            <template>
                <ul>
                    <!-- router-link sera renderizado como um tag <li> -->
                    <router-link to="/" tag="li"><a> Início </a></router-link>
                    <router-link to="/produtos" tag="li"><a> Produtos </a></router-link>
                </ul>
            </template>
            <script>
                export default {

                }
            </script>
            ```
    * `active-class`: utilizado para setar a classe css responsável pelo efeito ativo do link baseado na rota
        * `exact`: Para que ele só considera ativo quando a rota é exatamente a mesmo a não uma sub-rota
        - Ex:
            ```html
            <template>
                <ul>
                    <!-- Quando estiver na rota '/' ira aplicar o css atribuído a classe 'active' -->
                    <!-- O atributo 'exact' faz com que a classe só seja aplicado caso a rota seja exatamente '/' caso seja '/produtos' deixara de aplicar -->
                    <router-link to="/" tag="li" active-class="active" exact>
                        <a> Início </a></router-link>
                    <!-- Se aplicado o css na rota '/produtos' -->
                    <!-- Nesse caso sem o exact existindo qualquer outra rota '/produtos/qualquercoisa' continuara aplicando pois ainda contem o '/produtos' -->
                    <router-link to="/produtos" tag="li" active-class="active" >
                        <a> Produtos </a></router-link>
                </ul>
            </template>
            <script>
                export default {

                }
            </script>
            <style scoped>
                .active {
                    background-color: "#000";
                    color: "#FFF"
                }
            </style>
            ```
    * `name`: utilizado para definir um nome para a rota que pode ser utilizado para chamar a rota seja pelo `route-link` ou pela chamada via javascript
* O router também possibilita a chamada da rota de forma imperativa através do javascript
    - Ex:
        ```javascript
        import Vue from 'vue'
        import Router from 'vue-router'
        
        Vue.use(Router);

        const routes = [
            {
                path: '/',
                component: Home
            },
            {
                path: '/produtos'
                component: ProdutosList,
                name: 'listarProdutos'
            }
        ]
        ```
        ```html
            <template>
                <div>
                    <h1> Produtos </h1>
                    <button @click="goToInicio"> Início </button>
                </div>
            </template>
            <script>
                export default {
                    methods:{
                        goToInicio(){
                            // Considerando que o router tenha sido registrado na instância do vue
                            // Chamando a função 'push' ira redirecionar para a rota passada
                            //this.$router.push('/');
                            //this.$router.push({ path: '/' });
                            this.$router.push({ name: 'listarProdutos' });
                        }
                    }
                }
            </script>
        ```
        ```html
        <template>
            <ul>
                <!-- Quando estiver na rota '/' ira aplicar o css atribuído a classe 'active' -->
                <!-- O atributo 'exact' faz com que a classe só seja aplicado caso a rota seja exatamente '/' caso seja '/produtos' deixara de aplicar -->
                <router-link to="/" tag="li" active-class="active" exact>
                    <a> Início </a></router-link>
                <!-- Se aplicado o css na rota '/produtos' -->
                <!-- Nesse caso sem o exact existindo qualquer outra rota '/produtos/qualquercoisa' continuara aplicando pois ainda contem o '/produtos' -->
                <router-link to="/produtos" tag="li" active-class="active" >
                    <a> Produtos </a></router-link>
            </ul>
        </template>
        <script>
            export default {

            }
        </script>
        <style scoped>
            .active {
                background-color: "#000";
                color: "#FFF"
            }
        </style>
        ```
* As rotas filhas podem ser aninhadas na configuração com a propriedade `children` na configuração
    - Ex:
        ```javascript
        import Vue from 'vue'
        import Router from 'vue-router'
        
        Vue.use(Router);

        const routes = [
            {
                path: '/',
                component: Home
            },
            {
                path: '/produtos'
                component: ProdutosList,
                children: [
                    { path: '', component: ProdutosLista },
                    { path: ':id', component: ProdutosView }
                ]
            }
        ]
        ```
    * Para renderizar os componente filhos deve ser adicionado dentro do elemento pai um `router-view` indicando onde deve ser renderizado os filhos
        - Ex: 
            ```html
            <!-- Produtos.vue -->
            <template>
                <h1> Produtos </h1>
                <!-- Área onde sera renderizado os componentes filhos associado na configuração da rota -->
                <router-view />
            </template>
            [...]
            ```
    * Para navegar nos elementos filhos continua sendo com o a tag `router-link`
        - Ex:
            ```html
            <!-- ProdutosDetalhe.vue -->
            <template>
                <div>
                    Produto {{ id }}
                    <!-- <route-link tag="button" :to="`/produtos/${id}/editar`"> Editar </route-link> -->
                    <!-- Pode ser utilizado a forma passando um objeto para configurar a rota de destino do router-link -->
                    <route-link tag="button" :to="{ name: 'editarProduto', params: {id: id}, query: {} }"> Editar </route-link>
                </div>
            </template>
            <script>
                export default {
                    data() {
                        return {
                            props: ['id']
                        }
                    }
                }
            </script>
            ```           
            ```html
            <!-- Produtos.vue -->
            <template>
                <div>
                    <h1> Produtos </h1>
                    <ul>
                        <router-link to="/produtos/1" tag="li" > 
                            <a> Produto 1 </a>
                        </router-link>
                        <router-link to="/produtos/1" tag="li" > 
                            <a> Produto 1 </a>
                        </router-link>
                    </ul>
                    <button @click="goToInicio"> Início </button>
                </div>
            </template>
            <script>
                export default {
                    methods:{
                        goToInicio(){
                            // Considerando que o router tenha sido registrado na instância do vue
                            // Chamando a função 'push' ira redirecionar para a rota passada
                            this.$router.push('/');
                        }
                    }
                }
            </script>
            ```
            ```html
            <template>
                <ul>
                    <!-- Quando estiver na rota '/' ira aplicar o css atribuído a classe 'active' -->
                    <!-- O atributo 'exact' faz com que a classe só seja aplicado caso a rota seja exatamente '/' caso seja '/produtos' deixara de aplicar -->
                    <router-link to="/" tag="li" active-class="active" exact>
                        <a> Início </a></router-link>
                    <!-- Se aplicado o css na rota '/produtos' -->
                    <!-- Nesse caso sem o exact existindo qualquer outra rota '/produtos/qualquercoisa' continuara aplicando pois ainda contem o '/produtos' -->
                    <router-link to="/produtos" tag="li" active-class="active" >
                        <a> Produtos </a></router-link>
                </ul>
            </template>
            <script>
                export default {

                }
            </script>
            <style scoped>
                .active {
                    background-color: "#000";
                    color: "#FFF"
                }
            </style>
            ```

* Passagem de parâmetros pela rota
    * Para passar um valor como o *id* de um item pelo parâmetro deve ser anotado na rota o parâmetro `/path:parametro`
        - Ex:
            ```javascript
            import Vue from 'vue'
            import Router from 'vue-router'

            import Home from './components/Home'
            import Produtos from './components/Produtos'
            import ProdutosLista from './components/ProdutosLista'
            import ProdutosView from './components/ProdutosView'
            
            Vue.use(Router);

            const routes = [
                {
                    path: '/',
                    component: Home
                },
                {
                    path: '/produtos'
                    component: Produtos,
                    children: [
                        { path: '', component: ProdutosLista },
                        { path: ':id', component: ProdutosView }
                    ]
                },
                //{
                //    path: '/produtos:id'
                //    component: ProdutosView
                //}

            ]
            ```
    * O parâmetro passado pelo url pode ser acessado pelo variável `this.$route.params` disponível no contexto do vue
        * Para que o valor da variável seja alterado ao mudar para uma nova rota que tenha um parâmetro *id* deve ser passado por um *watch* a mudança
        - Ex: 
            ```javascript 
            // ProdutosView
            [...]
            export default {
                data() {
                    return {
                        id: this.$route.params.id;
                    }                    
                }
                watch: {
                    $route(to, from){
                        // Atualizando o id com o novo parâmetro vindo da nova rota.
                        this.id = to.$route.params.id;
                    }
                }
            }
            [...]
            ```
    * Os parâmetros em uma rota também podem ser passados para o componente como uma propriedade (props)
        * Adicionando ao objeto de configuração o `props: true`, todas os parâmetros serão passados como propriedade
        - Ex:
            ```javascript
            import Vue from 'vue'
            import Router from 'vue-router'
            
            Vue.use(Router);

            const routes = [
                {
                    path: '/',
                    component: Home
                },
                {
                    path: '/produtos'
                    component: ProdutosList,
                    children: [
                        { path: '', component: ProdutosLista },
                        { path: ':id', component: ProdutosView, props: true }
                    ]
                },
                //{
                //    path: '/produtos:id'
                //    component: ProdutosView,
                //    props: true
                //}
            ]
            ```
            ```html 
            <!-- ProdutosView -->
            <template>
                <div>
                    <span>id do produto: {{ id }}</span>
                </div>
            </template>
            <script>
            export default {
                props: ['id']
                //data() {
                //    return {
                //        id: this.$route.params.id;
                //    }                    
                //}
                //watch: {
                //    $route(to, from){
                //        // Atualizando o id com o novo parâmetro vindo da nova rota.
                //        this.id = to.$route.params.id;
                //    }
                //}
            }
            </script>
            ```
* router-view named [(Doc)](https://router.vuejs.org/guide/essentials/named-views.html)
* Router redirect, na configuração no **router.js** é possível com o atributo `redirect` definir uma rota de redireção baseado em um caminho
    - Ex:
        ```javascript
        import Vue from 'vue'
        import Router from 'vue-router'
        
        Vue.use(Router);

        const routes = [
            {
                path: '/',
                component: Home
            },
            {
                path: '/produtos'
                component: ProdutosList,
                children: [
                    { path: '', component: ProdutosLista },
                    { path: ':id', component: ProdutosView }
                ]
            },
            {
                path: '*', // Ira redirecionar todas as rota que não encontrar uma configuração para o '404.html'
                redirect: '/error/404.html'
            }
            //{
            //    path: '/redirect',
            //    redirect: '/usuarios'
            //}
        ]
        ```
    * scroll behavior [(Doc)](https://router.vuejs.org/guide/advanced/scroll-behavior.html#async-scrolling)
    * BeforeEach [(Doc)](https://router.vuejs.org/guide/advanced/navigation-guards.html#global-before-guards)
    * BeforeEnter [(Doc)](https://router.vuejs.org/guide/advanced/navigation-guards.html#global-before-guards)
    * beforeRouteEnter [(Div)](https://router.vuejs.org/guide/advanced/navigation-guards.html#in-component-guards)
    * beforeRouteLeave [(Doc)](https://router.vuejs.org/guide/advanced/navigation-guards.html#in-component-guards)
* Lazy load pelas rotas
    * Sendo uma boa pratica só carregar as partes que serão utilizadas
    * Para isso a parte de rotas do vue disponibilizada em conjunto com o webpack uma forma de configurar o carregamento o código lazy
        - Ex:
            - Ex:
            ```javascript
            import Vue from 'vue'
            import Router from 'vue-router'

            import Home from './components/Home'
            import Produtos from './components/Produtos'
            //import ProdutosLista from './components/ProdutosLista'
            //import ProdutosView from './components/ProdutosView'
            
            Vue.use(Router);

            const ProdutosLista = () => import('./components/ProdutosLista'); // Só sera carregado quando for acessado a roto para trazer esse componente
            const ProdutosView = () => import('./components/ProdutosView'); // Só sera carregado quando for acessado a roto para trazer esse componente

            const routes = [
                {
                    path: '/',
                    component: Home
                },
                {
                    path: '/produtos'
                    component: Produtos,
                    children: [
                        { path: '', component: ProdutosLista },
                        { path: ':id', component: ProdutosView }
                    ]
                },
                //{
                //    path: '/produtos:id'
                //    component: ProdutosView
                //}

            ]
            ```
        * O Lazy loading pode ocorrer por grupos de código, basta utilizar a anotação do webpack dando um nome para o arquivo 
          * `/* webpackChunkName: "produtos" */`
            - Ex:  
                ```javascript
                [...]
                // Quando for gerado os arquivo ira conter um com o nome de produto.js incapsulando o código dos componentes de forma agrupados
                const ProdutosLista = () => import(/* webpackChunkName: "produtos" */'./components/ProdutosLista');
                const ProdutosView = () => import(/* webpackChunkName: "produtos" */'./components/ProdutosView');
                [...]
                ```
## Vuex
* Controlador de estado
* Instalando: `npm install --save vuex`