# SED-Projeto-2
### A onda verde do Professor Kyller

O objetivo do projeto é modelar um sistema de semáforos e encontrar o ajuste ideal para que os veículos do tráfego de carros implementado parem no máximo uma vez durante o trajeto. Utilizando automatos temporizados e o software [UPPAAL](https://uppaal.org/).

Foi considerado o trajeto abaixo. Onde existem 4 semáforos que seguem as seguintes distâncias. 
- Distância do posto ao 1º semáforo: 1.200m
- Distância do 1º ao 2º semáforo: 130m
- Distância do 2º ao 3º semáforo: 160m
- Distância do 3º ao 4º semáforo: 170m
- Distância do 4º semáforo ao Rota: 90m
- Distância total: 1.750m.

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/17711cbf-e576-477c-b415-b1ec1c7251ae)

Ao estipular uma velocidade média de 40 km/h. Temos que cada carro gasta os seguintes tempos (aproximados) para fazer a travessia entre os semáforos.
- Tempo do posto ao 1º semáforo: 108s
- Tempo do 1º ao 2º semáforo: 12s
- Tempo do 2º ao 3º semáforo: 14s
- Tempo do 3º ao 4º semáforo: 15s
- Tempo do 4º semáforo ao Rota: 8s


Ao total foram criados 4 modelos de automatos para atingir os requistos solictados no projeto

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/2c6248ba-b23e-41af-ab7c-065222e9d0ae)


Dois dos automatos foram usados para controlar os demais. Os automatos do modelo Carro apenas respodem as ações realizadas em NewCar. E os automados do modelo semáforo respondem as ações realizadas em Controlador. Essas etapas seram detalhadas a seguir. 

### Automato Semáforo

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/662a36cd-8c72-435f-9ac8-9487963d4ccf)

Os quatro semáforos seguem o mesmo template mostrado acima. Todos iniciam verde. E fazem a transição de estados quando change (__broadcast chan__) ocorre, mas apenas se o clock local for maior que delay1, que representa o tempo estipulado para o semáforo permanecer verde. Assim como delay2 e delay3, tempo para amarelo e vermelho. __y >= delay1__, foi adicionado como condição, e __change?__ o método para sincronizar os semáforos. __broadcast chan__ pode ser utilizando de 1-N. 

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/e50d48bd-6a1f-4c4f-b60b-eeca4650f178)

Também é realizado o update de algumas variáveis. __y = 0__ para zerar o clock local, e __L[aux] = 1__ é responsável por bloquear a passagem dos carros pelo semáforo caso o mesmo não esteja mais no estado verde. As outras transições do template semáforo seguem o mesmo padrão descrito do verde para o amarelo. 

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/65d2c9f4-5750-440f-9f31-1a0958e2b03e)

Declaração dos semáforos utilizando system, e dos tempos de transição. Cada semáforo pode ter seus próprios tempos.


### Automato Controlador

Criado para realizar o controle principal do sistema, é responsável por incrementar a variável intTime para facilitar o acompanhamento do sistema, é possível incrementar em 5 segundos. O controlador também é responsável por induzir a alteração de estados dos semáforos (__change!__) caso eles atendam as condições impostas em guard (y>=delay). A varíavel z é o clock local do controlador, e ao fazer z == time1 o time global está passando também.  

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/c9b8b0e0-2400-4a5c-add7-711ecacf08e0)


### Automato Carro

Para criar o tráfego de carros, foi utilizado o automato carro. Onde a variável global N, representa quandos carros podem ser "gerados". A Variável N pode ser alterada para o número desejado. Mas para facilitar a visualização nos testes será utilizado 3 carros.  

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/6f436f8f-0c2c-4d96-b262-121e69c1bc3a)

A aproximação de um veículo no posto foi representada como um evento não controlável. E após isso uma variável registra o tempo de inicio do trajeto, para poder bloquear a passagem para o próximo estado até que o carro tenha chegado e que o semáforo da vez esteja verde. Distância_UP é uma função criada para que a variável dist1 assuma o valor, por exemplo de 108 + inicio, que seria o tempo necessáirio para o carro chegar ao próximo semáforo.  

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/0e4a1617-c99e-4d9b-a948-fb805f842abc)


### Automato NewCar

O automato New Car é apenas um complemento para a sincronização da aproximação de um carro no posto. 

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/75c6466b-f1c0-4a85-9c37-907f4aa2a180)


### Simulador

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/d63803e8-8313-427d-a4d4-d2c2d9d4301d)



