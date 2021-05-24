# Trabalho Final Circuitos Digitais 

Neste trabalho foi desenvolvido o Jogo da Velha pelo programa Quartus II da Altera.

## Implementação

A implementação do jogo da velha fez uso de displays de sete segmentos para exibir as marcações de cada jogador. Para implementação no display de sete segmentos, as marcações do jogo da velha foram adaptadas. Sendo as novas marcações:   

Circulo: representado pelo 0  
Xis: representado pelo 5  
Vazio: representado pelo 8  
  

Para a seleção de cada jogador foi utilizado o Interruptor Dip Rotativo (em inglês Rotary Dip Switch) **A6R-101RS**.   

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/rotary-dip.png" align="center" />    
    <div>Rotary Dip Switch A6R-101RS (<a href="https://omronfs.omron.com/en_US/ecb/products/pdf/en-a6r_rv.pdf">datasheet</a>) </div>
</div>  
  
  
O Rotary possui saídas de 0 a 9, sendo sua saída usada para marcação na matriz do jogo da velha.

A implementação do jogo foi feita separando em componentes cada processo.

## Componentes

### I. Auxiliares

Foi desenvolvido um conjunto de componentes auxiliares, componentes que foram usados diversas vezes dentro do circuito.

#### I.1. Multiplexador 2x1

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/mux2x1.png" align="center" />  
</div>  
Desenvolvido no laborátorio 01 ([lab01](https://www.inf.ufrgs.br/~mckrenn/lab01.pdf)), este mux foi usado muitas vezes dentro deste circuito.  

#### I.2. Memória 1 bit 
<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/memoria1bit.png" align="center" />  
</div>
Esta 1bit de memória foi desenvolvido para memorização de algumas saídas após cargas de entrada.  

#### I.3. Seletor
<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/seletor.png" align="center" />  
</div>
Este seletor foi usado para transformar a saída do controle em qual bit da memória está se referindo, sendo a tendo 9 saídas e somente uma delas saindo em 1, que é a correspondente a selecionada.  


#### I.4. Multiplexador 18x9

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/mux18x9.png" align="center" />  
</div>
Desenvolvido para uso com a memória de 9 bits de cada jogador  

### II. Controle
<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/controle.png" align="center" />  
</div>
Dentro do jogo o controle recebe em ATIVO uma AND que recebe VEZ_DO_JOGADOR e PARTIDA_ATIVA, sendo VEZ_DO_JOGADOR negado para o jogador 1.  

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/controle-interno.png" align="center" />  
    <div>Por dentro do circuito do controle</div>
</div>
Por dentro podemos ver que foi feito uma memorização da saída do controle do jogador, mudando somente quando o jogador estiver ativo e apertar novamente o botão.  

### III. Marcação do jogador
<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/marcacao-do-jogador.png" align="center" />  
</div>  
A marcação feita pelo jogador passa por um MUX que de acordo com de quem é a vez de jogador, retorna na sua saída a seleção do jogador.   


<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/marcacao-do-jogador-interno.png" align="center" />    
    Por dentro da marcação do jogador  
</div>  

Podemos ver que também é feita uma memorização da seleção do último jogador, sendo enviado o sinal de carga quando uma carga de entrada é recebida.

### IV. Verificação da Seleção

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/verificacao-selecao.png" align="center" />  
</div> 

Verificação recebe a memória (será vista no próximo item) dos jogadores 1 e 2 junto da seleção feita pelo jogador, se na memória de ambos o bit de numero dado pela seleção não estiver marcado então a cargaS é ativa.  

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/verificacao-selecao-interno.png" align="center" />  
    Por dentro  de verificação da marcação
</div>
Esta parte foi feita com um circuito sequencial que usa NANDs, pegando a seleção escolhida e se a seleção do mesmo bit da memória estiver em zero para ambos os jogadores, então é ativo a cargaS.

### V. Memória dos jogadores

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/memoria.png" align="center" />    
</div>
Este componente foi duplicado, sendo cada uma para cada jogador. A memória só receberá o sinal de carga de entrada quando uma seleção válida for enviada   

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/memoria-interno.png" align="center" />  
    Por dentro da memória (pedaço)
</div>  

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/memoria-interno-fim.png" align="center" /> 
    Por dentro da memória (pedaço final)
</div>  
Esta parte é a que envia o sinal de carga para a saída da memória, ela é ligada a uma OR que recebe o sinal de carga de entrada(cargaE) e a entrada de RESET.

### VI. Seleciona Memória

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/seleciona-memoria.png" align="center" /> 
</div>  
Esta parte recebe a memória de ambos jogadores, o sinal de carga saído da memória e a seleção do MUX é dada pelo VEZ_DO_JOGADOR  



<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/seleciona-memoria-interno.png" align="center" /> 
    Por dentro do Seleciona Memória
</div>  
A seleção dada pelo MUX fica acima na imagem, abaixo dela podemos ver que é feita a verificação de CAMPO_DISPONIVEL, retornando 1 se sim. Só mudando quando há uma entrada de carga.

### VII. Verificando se termina ou continua

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/checa-terminou.png" align="center" /> 
</div>

Esta parte recebe a saída do MUX, a saída de há campo disponivel e a carga dada pelo MUX18x9

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/checa-terminou-interno.png" align="center" /> 
    Por dentro do circuito Verifica se Venceu
</div>  
Podemos ver que a verificação é feita por um circuito sequencial que verifica se houve a marcação usando AND3 e enviando para uma OR8.

### VIII. Visores

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/visores.png" align="center" />    
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/visores-interno.png" align="center" />   
    Por dentro do componente Visores
</div>
Visores é um conjunto de outros visores, sendo repetido o processo para cada bit de cada jogador

#### VIII.2. Visor

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/visor-interno.png" align="center" /> 
</div>
Podemos ver que há uso de portas lógicas para se chegar a resposta se o visor deve estar 8, 5 ou 0.

Para ver por dentro do decodificador de 7 segmentos ([lab03](https://www.inf.ufrgs.br/~mckrenn/lab03.pdf))

## Simulações

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/simulacao1.png" align="center" /> 
    Simulação com teste apertando botão antes da partida iniciar e com jogador 2 ganhando
</div>

<div align="center">
    <img src="https://github.com/mthcsta/jogo-da-velha/blob/master/readme_content/simulacao2.png" align="center" /> 
    Simulação com uma simples partida e com jogador 2 ganhando novamente
</div>