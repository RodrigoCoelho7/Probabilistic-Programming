~~~~
//Exercício 1

//Lançar uma moeda ao ar n vezes e contar quantas vezes saiu cara
//True=cara e False=coroa

var moeda = function() {
  return flip(0.5)
}

//True=cara, false=coroa
var lancamentos = repeat(10,moeda)
viz(lancamentos)


var resultados = sum(lancamentos)
console.log("Saíram " + resultados + " caras")

//Retorna verdadeiro se forem todos coroas e falso caso contrário
var TodosCoroas = all(function(x){return !x},lancamentos)
console.log("É " + TodosCoroas + " que tenham saído todos coroas")

var TodosIguais = function (){
  if ((resultados == lancamentos.length) || (resultados == 0)){
    console.log("Os lançamentos foram todos iguais")
    //return true
  }
  else{
    console.log("Os lançamentos não foram todos iguais")
    //return false
  }
}

TodosIguais()
~~~~

~~~~
//Exercício 2

var moeda = function() {
  return flip(0.5)
}


//True=cara, false=coroa
var lancamentos = repeat(1000,moeda)
viz(lancamentos)
~~~~

~~~~
//Exercício 3

var make_coin = function(prob) {
  return function() {
    return flip(prob)
  }
}

var moeda = make_coin(0.7)
var moeda2 = make_coin(0.2)
var moeda3 = make_coin(0.9)

//True=cara, false=coroa
var lancamentos = repeat(1000,moeda)
viz(lancamentos)
viz.table(lancamentos)

var lancamentos2 = repeat(1000,moeda2)
viz(lancamentos2)
viz.table(lancamentos2)

var lancamentos3 = repeat(1000,moeda3)
viz(lancamentos3)
viz.table(lancamentos3)
~~~~

~~~~
//Exercício 4

//Parte 1 do exercicio

var make_coin = function(prob) {
  return function() {
    return flip(prob)+flip(prob)+flip(prob)+flip(prob)+flip(prob)
  }
}

var moedas = make_coin(0.7)

//True=cara, false=coroa
//Histograma com o número de caras
var lancamentos = repeat(10,moedas)
viz(lancamentos)

//Parte 2 do exercicio usando a distribuição binomial

var make_coin = function(prob) {
  return function() {
    return binomial({p:prob,n:5})
  }
}

var moedasf = make_coin(0.7)

var lancamentosdist = repeat(10,moedasf)
viz(lancamentosdist)
~~~~

~~~~
//Exercício 5

var valores = [1,2,3,4,5,6]

var dado = function(){
  return categorical({vs:valores})
}

var resultados = repeat(1,dado)
viz(resultados)

console.log("Saiu o numero " + resultados + " no lancamento do dado")

viz.table(Infer(dado))
~~~~

~~~~
//Exercício 6

var valores = [1,2,3,4,5,6]

var dado = function(){
  return categorical({vs:valores})
}

var modelo = function(){
  var d1 = dado()
  var d2 = dado()
  return d1+d2
}

var lancamentos = repeat(1000,modelo)
viz(lancamentos)
~~~~

~~~~
//Exercício 7

//C-Copas
//E-espadas
//O-Ouros
//P-Paus
//Exemplos: 10O = 10 de Ouros; KE = Rei de espadas

//Criar um array que contém todas as cartas
var valores = ["2","3","4","5","6","7","8","9","10","J","Q","K","A"]
var naipes = mapN(function(x){return "C"},13).concat(mapN(function(x){return "E"},13)).concat(mapN(function(x){return "O"},13)).concat(mapN(function(x){return "P"},13))
var concat = function(x, y) { return x + y; };
var cartas = map2(concat,valores.concat(valores).concat(valores).concat(valores),naipes)


var carta = function(){
  return categorical({vs:cartas})
}

var sorteio = carta()
console.log("Saiu a carta " + sorteio)
~~~~

~~~~
//Exercício 8

//Criar um array que contém todas as cartas
var valores = ["2","3","4","5","6","7","8","9","10","J","Q","K","A"]
var naipes = mapN(function(x){return "C"},13).concat(mapN(function(x){return "E"},13)).concat(mapN(function(x){return "O"},13)).concat(mapN(function(x){return "P"},13))
var concat = function(x, y) { return x + y; };
var cartas = map2(concat,valores.concat(valores).concat(valores).concat(valores),naipes)

//Uma carta é sorteada, descobre-se o indice dessa carta e 
// remove-se a carta do array usando as funções pré-definidas indexOf e splice
var carta = function(){
  var sorteio = categorical({vs:cartas})
  var carta_a_remover = cartas.indexOf(sorteio, 0)
  cartas.splice(carta_a_remover,1)
  return sorteio
}

//Só se pode repetir um máximo de 52 vezes, porque depois não há mais cartas
var resultados = repeat(52,carta)
viz(resultados)
~~~~

~~~~
//Exercício 8 alternativo
//Forma diferente de fazer o ex alterando a probabilidade de uma carta já saída para 0
// em vez de remover a carta da lista

//Cria uma lista com as probabilidades de sair cada carta: Todas têm uma prob de 1/52
var lista_prob = mapN(function (x){return 1/52},52)


//Cria uma lista com as cartas todas
var valores = ["2","3","4","5","6","7","8","9","10","J","Q","K","A"]
var naipes = mapN(function(x){return "C"},13).concat(mapN(function(x){return "E"},13)).concat(mapN(function(x){return "O"},13)).concat(mapN(function(x){return "P"},13))
var concat = function(x, y) { return x + y; };
var cartas = map2(concat,valores.concat(valores).concat(valores).concat(valores),naipes)

//Vê qual foi a carta que foi sorteada e substitui a probabilidade de sair no futuro por 0
//A função splice também pode trocar elementos de uma lista
var carta = function(){
  if (all(function (x){return x==0},lista_prob)) {return null}
  var valor = categorical({ps:lista_prob,vs:cartas})
  var carta_a_remover = cartas.indexOf(valor, 0)
  lista_prob.splice(carta_a_remover,1,0)
  return valor
}


var resultados = repeat(52,carta)
viz(resultados)

//saiu apenas uma carta cada em 52 lançamentos, logo o código funciona
~~~~