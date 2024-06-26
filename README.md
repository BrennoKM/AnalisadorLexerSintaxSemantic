# Analisador Léxico, Sintático e Semântico

Esse projeto tem com objetivo reconhecer a linguavel OWL no modelo Manchester Syntax

## Como preparar o ambiente? 👵
- ### Baixando dependências (opcional caso já as possua instaladas)
      sudo apt update
      sudo apt upgrade
      sudo apt install g++ gdb
      sudo apt install make cmake
      sudo apt install flex libfl-dev
      sudo apt install bison libbison-dev

- ### Agora já podemos executar os comandos para compilar o executavel do projeto!
  **Executar o comando "make" caso conheça o funcionamento dele, caso contrário, podemos executar os comandos um a um**

  1. Gerar o arquivo do analisador léxico com o flex:

         flex __lexer.l

  2. Gerar o arquivo do analisador sintático com o bison:
   
	     bison -d __sintax.y -Wcounterexamples 

  3. Compilar tudo em no executavel do projeto:
   
	     g++ lex.yy.c __sintax.tab.c -std=c++17 -o executavel

  4. Limpar arquivos desnecessários (opcional):
   
	     rm lex.yy.c __sintax.tab.c __sintax.tab.h

## Com o executavel em mãos, podemos fazer uso dele da seguinte maneira
	./executavel < arquivo.txt
   > sendo "arquivo.txt" um arquivo de texto que serve pra de entrada

  ### Alguns arquivos de testes já estão definidos no projeto, sendo eles:
    _owl.txt	
   > Esse arquivo está repleto de erros na sua gramática e também erros semânticos.
	
	_owl2.txt
   > Esse arquivo é um exemplo do arquivo _owl.txt corrigido, sem erros sintáticos e sem erros semânticos.

	_owl3.txt
   > Esse é um arquivo pequeno que eu recomendo para fazer testes rápidos do projeto.
	
 	_owl4.txt
   > A mesma coisa do anterior

	_owl5.txt
   > Esse é referente ao ultimo arquivo disponibilizado da gramática do Manchester Syntax, sendo o exemplo mais complexo dentre todos.

## Leve explicação da gramática desenvolvida
- As variáveis não-terminais sempre deviram em um ou mais não-terminais, exceto aos casos especiais, dos quais os não-terminais derivam unicamente um token terminal.
	> Ahnnn?
	
	Exemplo: a várivel "orand" deriva em "or" ou "and", sendo que "or" deriva o token "OR" e and deriva o token "AND".
	
	orand: or  
	    | and                 
	    ;
	
	or: OR
	;
	
	and: AND
	;
	
	Por quê? Isso deixa a gramática visualmente mais extensa, mas em troca, permite que cada token possa ser tratado de uma forma diferente de forma eficiente.

- Outra peculiaridade é que a leitura é feita da direita pra esquerda.
	> Ahnnnnnnnnnnn???????
	
	Exemplo: ao invés de usar 'indivi virgula individuos' foi escolhido a forma descrita abaixo.
	
	individuos: indivi              
	  | individuos virgula indivi
	  ;
	
	Por quê? Isso deixa a gramática confusa, mas em troca, permite que os tokens lidos sejam impresso no terminal na ordem em que são lidos.

## Analise Semântica
- A analise semântica foi feita em conjunto com a análise sintática com o auxilio de flags e estruturas de dados para que sejam feitas verificações em pontos desejados.
>Exemplo de exibição de um erro semântico: ![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/13458208-788f-4795-9847-3b9ea5ff9a03)


## Exemplos de entrada
----------------------------------------------------------------------------------------------------------------
![image](https://github.com/BrennoKM/AnalisadorLexerSintax/assets/99992197/46915f3f-879c-43d5-b4a8-5a473d65b743)

Saida: ![image](https://github.com/BrennoKM/AnalisadorLexerSintax/assets/99992197/dc12b728-e9c3-49cf-9232-dcaca7511521)
----------------------------------------------------------------------------------------------------------------
### Exemplo 1: problema na gramática
![image](https://github.com/BrennoKM/AnalisadorLexerSintax/assets/99992197/91e3a819-03d5-450f-9cf4-f0764d22e5f4)

Saida: ![image](https://github.com/BrennoKM/AnalisadorLexerSintax/assets/99992197/5e2e517b-468f-47a6-aa44-b34a7889f30b)
----------------------------------------------------------------------------------------------------------------
### Exemplo 2: erro semântico
![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/361c1ed6-971e-40ba-8a02-89911b4fae3b)
> Note que o axioma de fechamento possui MozzarellaTopping, mas em nenhum momento prévio ele foi declarado com essa propiedade.

Saida: ![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/507f0007-b617-4743-9674-fd16f0a6f5c3)
----------------------------------------------------------------------------------------------------------------
### Exemplo 3: erro semântico
![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/547d7e0a-f141-4caf-9fab-f7a93a8b72f2)
> Note que o axioma de fechamento possui MozzarellaTopping, a classe possui declarações com MozzarellaTopping, mas com uma propiedade diferente da usada no axioma de fechamento.

Saida: ![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/fa1c0abf-487c-40f1-9ffd-2d90d588ca0e)
----------------------------------------------------------------------------------------------------------------
### Exemplo 4: erro semântico
![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/fee565e6-ae6e-4399-a808-eb2de279aa4a)
> Pro analisador sintático, no exemplo acima não será encontrado nenhum problema, mas é fácil observar que os tipos de dados estão incoerentes com as cardinalidades.

Saida: ![image](https://github.com/BrennoKM/AnalisadorLexerSintaxSemantic/assets/99992197/1e7ac73a-b0f5-4ee2-b60d-643c3efd6487)
----------------------------------------------------------------------------------------------------------------
