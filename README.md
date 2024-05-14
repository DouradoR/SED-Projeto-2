# SED-Projeto-2
### A onda verde do Professor Kyller

O objetivo do projeto é modelar um sistema de semáforos e encontrar o ajuste ideal para que os carros parem no máximo uma vez durante o trajeto. Utilizando automatos temporizados e o software [UPPAAL](https://uppaal.org/).

E Considerando o trajeto abaixo. Onde existem 4 semáforos que seguem as seguintes distâncias. 
- Distância do posto ao 1º semáforo: 1.200m
- Distância do 1º ao 2º semáforo: 130m
- Distância do 2º ao 3º semáforo: 160m
- Distância do 3º ao 4º semáforo: 170m
- Distância do 4º semáforo ao Rota: 90m
- Distância total: 1.750m.

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/17711cbf-e576-477c-b415-b1ec1c7251ae)

Ao considerar uma velocidade média de 40 km/h. Temos que cada carro gasta os seguintes tempos (aproximados) para fazer a travessia entre os semáforos.
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

Criado para realizar o controle principal do sistema, é responsável por incrementar a variável intTime para facilitar o acompanhamento do sistema, é possível incrementar 10 segundos ou 5 segundos. O controlador também é responsável por induzir a alteração de estados dos semáforos (__change!__) caso eles atendam as condições impostas em guard (y>=delay). A varíavel z é o clock local do controlador, e ao fazer z == timeX o time global está passando também.  

![image](https://github.com/DouradoR/SED-Projeto-2/assets/86689951/c9b8b0e0-2400-4a5c-add7-711ecacf08e0)


### Automato Carro

### Automato NewCar

