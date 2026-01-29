# Zephyr
1 - o que é uma device tree?
Device tree é uma estrutura de dado hierarquica que define a configuração do projeto, o Zephyr utiliza para listar o hardware disponivel e sua configuração inicial, para placas suportadas.

2 - o que é uma device tree source?
contem a forma humanamente legivel dos conteudos, ela utiliza o DTS.

 - Exemplo:
 ```
	/ {
		a-node {
			subnode_nodelabel: a-sub-node {
				foo = <3>;
			};
		};
	};
 ```
- Onde / denota um nodo raiz
- a-node é um nodo qualquer filho da raiz, ele pode ter qualquer nome
- a-sub-node é filha de a-node e pode ter qualquer nome.
- subnode_nodelabel é uma label que serve para referenciar um nodo, e cada nodo pode ter nenhuma, um ou varias labels.
- o path de um nodo é a concatenação dos outros nodos entao o a-sub-node tem o path de /a-node/a-sub-node, onde o primiro / é o nodo raiz.
- nodos podem ter propriedades.


3 - o que é uma device tree binding?
bindings descrevem tipos de dado e conteudos presentes na device tree, algo que a source não consegue fazer

4 - o que é um node?
é uma definição das capacidades do hardware juntamente com propriedades e outras informações que permitem referenciar e configurar.

5 - o que é a Kconfig?



6 - como a device tree source e binding trabalham juntas?
a uniao da source e binding gera um catologo generico em C que descreve a estrutura da arvore e como ela interage com o software. tambem permite a interface com o devicetree.h

7 - o que sao as propriedades de um nodo?
Sao valores armazenados, eles no geral sao utilizados para configurar ou descrever o nodo.
algumas propriedades sao importantes pois definem propriedades padroes:
- Compatible(String): nome do hardware que o dispositivo representa, por exemplo vendor,device que é o formato recomendado.
- reg(Address,Int) informaçao usuada para endereçar o dispositivo, funciona como um sequencia de pares definidos por endereço tamanho chamados de register block, convencionalmente utilizam hex.
- status(String) diz se o nodo esta funcionando ou algum outro status relevante. os mais comuns sao "okay" e "disabled",
- Interrupts definido pelo hardware, cada interrupt tem um especificador e um numero de celulas.

8 - quais tipos de dados sao aceitos nas propriedades?
- String "algo aqui"
- int <1>, sempre precisa estar entre <>
- boolean, para declarar true apenas declare a variavel, caso queira false, apaguea
- array <1 2 3> conteudo separado por espaço
- uint8-array [00 01 ab]
- string-array "strings", "outra string" 
- phandle = <&meuNodo>;
- phandles = <&meuNodo &meuNodo2 &meuNodo3>
- phandle-array = <phandle ou phandles>, <...>
* sempre terminado em ; 

9 - o que sao pHandles?
Sao parecidos com ponteiros, e servem para referenciar outros nodos. tem varias formas de serem referenciados mas não parece relevante pro projeto atual.

10 - quais sao os input files?
- sources(dts)
- includes(dtsi)
- overlay(.overlay)
- bindings(.yaml)

um exemplo de devicetree com esses arquivos
boards/<ARCH>/<BOARD>/<BOARD>.dts
dts/common/skeleton.dtsi
dts/<ARCH>/.../<SOC>.dtsi
dts/bindings/.../binding.yaml

toda placa tem um BOARD.dts onde board é o nome da placa
um .dts pode incluir um ou mais dtsis.
o dtsi descreve a CPU ou IC. um dtsi pode incluir outros dtsi
o processo de inclusão é igual a c #include <filename>

um overlay extende ou modifica o .dts
o overlay pode desativar ou ativar perifericos ou sensores, alem de reconfigurar o kernel
overlays tambem sao usados com shields
.overlays sao carregados automaticamente de alguns locais, mas podem ser especificados pelo DTC_OVERLAY_FILE quando fazemos a build.

os .yaml juntam todos os outros arquivos e descrevem como os C macros e outros sao construidos.