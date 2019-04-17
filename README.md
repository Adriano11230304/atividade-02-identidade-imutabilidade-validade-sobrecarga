# atividade-02-identidade-imutabilidade-validade-sobrecarga

_Classroom link:_ <https://classroom.github.com/a/Ti-_ZVrw>

## Modelar e implementar respeitando os princípios de Orientação a Objetos

[strong, firm, focus, always concentrate ...](http://youtu.be/PWiipjG7NEU)

#### Assuntos

- identidade
- métodos `toString` e `equals`
- imutabilidade
- sobrecarga
- validade (exceptions)

#### Prazo: 2019-04-21 Peso: 1.2 pts

**Restrição: não podem ser usadas as bibliotecas do Java, por exemplo, a classe `Math`, `Scanner`, etc, inclusive os métodos de Integer, como `parseInt` ou métodos de String, EXCETO `length`, `charAt` e `equals`; Os Casos de Teste podem ser corrigidos, mas a especificação não pode ser alterada.**

### Implementar o objeto `Coord` (0.5 pts, substitui `Time`)

Instâncias de `Coord` devem representar uma posição geográfica no formato de _latitude_ e _longitude_ em graus decimais, sendo que a _latitude_ vai de `-90.0` a `+90.0` e a _longitude_ de `-180.0` a `+180.0`. A construção sem argumentos de uma coordenada deve instanciar _latitude_ `0` e _longitude_ `0`. Após a construção não devem ser permitidas alterações na _latitude_ e _longitude_ a não ser que outra instância seja construída, em outras palavras, os objetos devem ser **imutáveis**.

Considere os Casos de Teste:
```java
// construtores:
Coord c1 = new Coord();
System.out.println(c1.getLat() == 0.0);
System.out.println(c1.getLong() == 0.0);

Coord c2 = new Coord(50.0, 134.0);
System.out.println(c2.getLat() == 50.0);
System.out.println(c2.getLong() == 134.0);

Coord c3 = new Coord(-90.0, -180.0);
System.out.println(c3.getLat() == -90.0);
System.out.println(c3.getLong() == -180.0);

// estas coordenadas são inválidas e devem lançar exceção
// faça serem rejeitadas e depois comente-as para não parar o programa
Coord e1 = new Coord(-91.0, 0.0);
Coord e2 = new Coord(100.0, 0.0);
Coord e3 = new Coord(10.0, -182.0);
Coord e4 = new Coord(10.0, 200.0);
Coord e5 = new Coord(-95.0, -200.0);

// imutabilidade: as linhas a seguir devem causar erro de compilação
// verifique se está de acordo e depois comente-as
Coord c4 = new Coord();
c4.getLat() = 30.0;  // não deve permitir reatribuição
c4.getLong() = 80.0; // não deve permitir reatribuição

// operações/comandos:
Coord in = new Coord(30.0, 50.0);
Coord out = in.moveNorth(5.0); // deslocamento
System.out.println(in.getLat() == 30.0); // deve ser imutável
System.out.println(out.getLat() == 35.0);
out.moveNorth(5.0); // sem reatribuição sem alteração
System.out.println(out.getLat() == 35.0);
out = out.moveNorth(5.0); // reatribuindo
System.out.println(out.getLat() == 40.0);
out = out.moveSouth(60.0);
System.out.println(out.getLat() == -20.0);
out = out.moveSouth(30.0);
System.out.println(out.getLat() == -50.0);
out = out.moveSouth(-10.0);
System.out.println(out.getLat() == -40.0);
out = out.moveNorth(-10.0);
System.out.println(out.getLat() == -50.0);
System.out.println(out.getLong() == 50.0);
out = out.moveEast(50.0);
System.out.println(out.getLong() == 100.0);
out = out.moveWest(180.0);
System.out.println(out.getLong() == -80.0);
out = out.moveWest(-10.0);
System.out.println(out.getLong() == -70.0);
out = out.moveEast(-10.0);
System.out.println(out.getLong() == -80.0);
// até aqui 0.2

// consultas:
Coord q = new Coord();
System.out.println(q.getLat() == 0);
System.out.println(q.getLong() == 0);
System.out.println(q.isEquatorLine() == true);
System.out.println(q.isMeridianLine() == true);
q = q.moveNorth(10.0);
System.out.println(q.getLat() == 10);
System.out.println(q.isEquatorLine() == false);
q = q.moveEast(10.0);
System.out.println(q.isMeridianLine() == false);
q = q.moveEast(170.0);
System.out.println(q.getLong() == 180.0);
System.out.println(q.isMeridianLine() == true);
q = q.moveWest(200.0);
System.out.println(q.getLong() == -20.0);
System.out.println(q.isMeridianLine() == false);
q = q.moveWest(160.0);
System.out.println(q.getLong() == -180.0);
System.out.println(q.isMeridianLine() == true);

Coord r = new Coord(30.0, 70.0);
System.out.println(r.getLat() == 30.0);
System.out.println(r.getLong() == 70.0);
System.out.println(r.isNorth() == true);
System.out.println(r.isSouth() == false);
System.out.println(r.isOrient() == true);
System.out.println(r.isOcident() == false);
r = r.moveWest(140.0).moveSouth(60.0);
System.out.println(r.getLat() == -30.0);
System.out.println(r.getLong() == -70.0);
System.out.println(r.isNorth() == false);
System.out.println(r.isSouth() == true);
System.out.println(r.isOrient() == false);
System.out.println(r.isOcident() == true);

// toString:
System.out.println(c1.toString().equals("0.0°, 0.0°"));
System.out.println(c2.toString().equals("50.0°, 134.0°"));
System.out.println(c3.toString().equals("-90.0°, -180.0°"));
System.out.println(out.toString().equals("-50.0°, -80.0°"));
System.out.println(q.toString().equals("10.0°, -180.0°"));
System.out.println(r); // -30.0°, -70.0°
System.out.println(r.toString().equals("-30.0°, -70.0°"));

Coord t = new Coord(-32.0714021, -52.1425059);
// https://www.google.com.br/maps/@-32.0714021,-52.1425059,13z?hl=pt-BR
System.out.println(t);; // -32.0714021°, -52.1425059°
System.out.println(t.toString().equals("-32.0714021°, -52.1425059°"));

// equals:
System.out.println(t.equals(r) == false);
Coord y = new Coord(-32.0714021, -52.1425059);
System.out.println(t.equals(y) == true);
System.out.println(y.equals(t) == true);
System.out.println(y.equals(y) == true);
System.out.println(y.equals(r) == false);

// desafio: escrever método que retorna link para Google Maps!
Coord t = new Coord(37.402473, -122.3212843);
String url = t.getGoogleMapsURL();
System.out.println(url.equals("https://www.google.com.br/maps/@37.402473,-122.3212843,10z?hl=pt-BR"));
// descomente para ver se funciona (não testei)
// Runtime.getRuntime().exec("google-chrome " + url);
// tente outro navegador, no windows tente "start " + url
```

**Informações adicionais:**

- Imagem esclarecedora <http://eogn.com/images/newsletter/2014/Latitude-and-longitude.png>
- Sobre coordenadas decimais <http://opussig.blogspot.com.br/2013/01/coordenadas-geograficas-em-formato.html>
- Sessão _remember_ <http://brasilescola.uol.com.br/geografia/coordenadas-geograficas.htm>

### Implementar uma classe para gerar objetos imutáveis de Tempo e suas operações (0.5 pts)

Considere um instante no tempo em horas, minutos e segundos, entre `00:00:00` e `23:59:59`. Implementar construtores e métodos para lidar com esse tempo de maneira *fail-safe* (sem rejeitar as entradas, mas adaptando-as). A API (interface do objeto) será implementada na língua inglesa com construtores para `h:m:s`, `h:m` e somente `h`. Os métodos disponíveis serão `Time#plus(Time):Time`, `Time#plusHours(int):Time`, `Time#plusMinutes(int):Time`, `Time#plusSeconds(int):Time`, `Time#minus(Time):Time`, `Time#minusHours(int):Time`, `Time#minusSeconds(int):Time`, `Time#tick():Time` (adiciona um segundo), `Time#shift():Time` (inverte o turno),`Time#isMidDay():boolean` (questiona se é meio-dia) e `Time#isMidNight():boolean` (questiona se é meia-noite).

**Regra específica**: a classe `Time` deve ter **apenas um atributo** `int` constante (`final`).

Casos de teste:

```java
Time t1 = new Time();
// representação string, padrão 00:00:00
System.out.println(t1.toString().equals("00:00:00"));
Time t2 = new Time(1, 40, 5);
System.out.println(t2.toString().equals("01:40:05"));
Time t3 = t1.plus(t2);
System.out.println(t3.toString().equals("01:40:05"));
System.out.println(t3.getHours() == 1);
System.out.println(t3.getMinutes() == 40);
System.out.println(t3.getSeconds() == 5);
// deve ser imutável
System.out.println(t1.getHours() == 0);
System.out.println(t1.getMinutes() == 0);
System.out.println(t1.getSeconds() == 0);
// plus (mais) e equals
Time t4 = t3.plus(t2);
System.out.println(t4.toString().equals("03:20:10"));
System.out.println(t4.equals(new Time(3, 20, 10)));
Time t5 = t2.plusHours(1);
System.out.println(t5.equals(new Time(2, 40, 5)));
Time t6 = t4.plusHours(23);
System.out.println(t6.equals(new Time(2, 20, 10)));
Time t7 = t6.plusMinutes(10);
System.out.println(t7.equals(new Time(2, 30, 10)));
Time t8 = t7.plusSeconds(80);
System.out.println(t8.equals(new Time(2, 31, 30)));
// até aqui 0.2

// métodos consulta
Time t9 = new Time().plusHours(19).plusMinutes(23).plusSeconds(18);
System.out.println(t9.toString().equals("19:23:18"));
Time t10 = t9.plusHours(-1).plusMinutes(-1).plusSeconds(-1);
System.out.println(t10.toString().equals("18:22:17"));
Time t11 = t10.minusHours(2).minusMinutes(2).minusSeconds(2);
System.out.println(t11.toString().equals("16:20:15"));
Time t12 = t11.minusHours(-5);
System.out.println(t12.toString().equals("21:20:15"));
Time t13 = t11.minus(t12);
System.out.println(t13.toString().equals("19:00:00"));
System.out.println(t13.isMidDay() == false);
Time t14 = t13.minus(t13);
System.out.println(t14.toString().equals("00:00:00"));
System.out.println(t14.isMidDay() == false); // é meio-dia?
System.out.println(t14.isMidNight() == true); // é meia-noite?
System.out.println(t14.plusHours(12).isMidDay() == true);
Time t15 = new Time(3, 40);
System.out.println(t15.toString().equals("03:40:00"));
Time t16 = t15.shift(); // inverte turno
System.out.println(t16.toString().equals("15:40:00"));
Time t17 = t16.shift();
System.out.println(t17.toString().equals("03:40:00"));
Time t18 = t17.tick();
System.out.println(t18.toString().equals("03:40:01"));
Time t19 = t18.tick().tick().tick(); // cada tick um segundo
System.out.println(t19.toString().equals("03:40:04"));
Time t20 = t19.plusHours(50).plusMinutes(50).minusSeconds(50).tick().shift();
System.out.println(t20.toString().equals("quanto vale t20? escreva aqui"));
// até aqui 0.4

// situações especiais
Time esp = new Time(-1, 40, 5); // 24 - 1
System.out.println(esp); // 23:40:05
System.out.println(esp.toString().equals("23:40:05"));
esp = new Time(12, -15, -5);
System.out.println(esp.toString().equals("11:44:55"));
esp = new Time(25, 70, 70);
System.out.println(esp.toString().equals("02:11:10"));
esp = new Time(-28, 10, 20); // 24 - 28 = -4, 24 - 4
System.out.println(esp.toString().equals("20:10:20"));
// até aqui 0.5
```

### Implementar a ideia de _Tempo Decorrido_ (0.4 pts)

Em português _"time span"_ seria **intervalo de tempo**. Assim, ele pode representar um tempo em dias, horas, minutos e segundos. `TimeSpan` deve ser imutável.

Considere os Casos de Teste:

```java
// construtores
TimeSpan ts1 = new TimeSpan(7, 3, 45, 35);
System.out.println(ts1.getDays() == 7);
System.out.println(ts1.getHours() == 3);
System.out.println(ts1.getMinutes() == 45);
System.out.println(ts1.getSeconds() == 35);
ts1 = new TimeSpan(8, 12, 9);
System.out.println(ts1.getDays() == 0);
System.out.println(ts1.getHours() == 8);
System.out.println(ts1.getMinutes() == 12);
System.out.println(ts1.getSeconds() == 9);
ts1 = new TimeSpan(4, 18, 110); // entradas adaptáveis
System.out.println(ts1.getDays() == 0);
System.out.println(ts1.getHours() == 4);
System.out.println(ts1.getMinutes() == 19);
System.out.println(ts1.getSeconds() == 50);
ts1 = new TimeSpan(4, 68, 110); // entradas adaptáveis
System.out.println(ts1.getDays() == 0);
System.out.println(ts1.getHours() == 5);
System.out.println(ts1.getMinutes() == 9);
System.out.println(ts1.getSeconds() == 50);
// até aqui 0.1

// validade e exceções
try {
  ts1 = new TimeSpan(-1, 4, 68, 110);
  // se essa linha for impressa uma exceção não foi lançada
  // falhando no caso de teste
  System.out.println(false);
} catch (IllegalArgumentException e) {
  System.out.println(true); // se for impresso a exceção foi lançada! ok!
  System.out.println(e.getMessage()); // Can't be negative
}
try {
  ts1 = new TimeSpan(1, -4, 68, 110); System.out.println(false);
} catch (IllegalArgumentException e) {
  System.out.println(true);
}
try {
  ts1 = new TimeSpan(1, 4, -68, 110); System.out.println(false);
} catch (IllegalArgumentException e) {
  System.out.println(true);
}
try {
  ts1 = new TimeSpan(1, 4, 68, -110); System.out.println(false);
} catch (IllegalArgumentException e) {
  System.out.println(true);
}
try {
  ts1 = new TimeSpan(0, 0, 0, 0); System.out.println(false);
} catch (IllegalArgumentException e) {
  System.out.println(e.getMessage()); // Can't be zero
  System.out.println(true);
}
// até aqui 0.2

// toString
System.out.println(ts1); // deve imprimir 5 hours, 9 minutes e 50 seconds
System.out.println(ts1.toString().equals("5 hours, 9 minutes e 50 seconds"));
TimeSpan ts2 = new TimeSpan(1, 12, 45, 1);
System.out.println(ts2.toString().equals("1 day, 12 hours, 45 minutes e 1 second"));
TimeSpan ts3 = new TimeSpan(0, 0, 0, 15);
System.out.println(ts3.toString().equals("15 seconds"));
ts3 = new TimeSpan(0, 1, 0, 0);
System.out.println(ts3.toString().equals("1 hour"));
ts3 = new TimeSpan(0, 0, 25, 0);
System.out.println(ts3.toString().equals("25 minutes"));
// toString em português
System.out.println(ts1.toString("pt").equals("5 horas, 9 minutos e 50 segundos"));
System.out.println(ts2.toString("pt")); //1 dia, 12 horas, 45 minutos e 1 segundo
System.out.println(ts2.toString("pt").equals("1 dia, 12 horas, 45 minutos e 1 segundo"));
System.out.println(ts3.toString("pt").equals("25 minutos"));
// até aqui 0.3

// equals, greaterThan, lessThan (igual, maior que, menor que)
TimeSpan ts4 = new TimeSpan(1, 12, 45, 1);
System.out.println(ts2.equals(ts4) == true);
System.out.println(ts4.equals(ts2) == true);
System.out.println(ts4.equals(ts3) == false);
System.out.println(ts4.greaterThan(ts3) == true);
System.out.println(ts4.lessThan(ts3) == false);
System.out.println(ts3.lessThan(ts2) == true);
System.out.println(ts4.lessThan(ts2) == false);
System.out.println(ts4.greaterThan(ts2) == false);
// até aqui 0.4
```

### Ponto (0.3 pts)

Considere um sistema para bater ponto. Nessa fase de desenvolvimento ele não é muito complexo, basta informar o nome do funcionário para abrir um ponto e em seguida "bater" o ponto para registrar entrada e saída. Para realizar essa parte tu podes associar o `Ponto` ao `Time` e `TimeSpan`. Será necessário, claramente, calcular o intervalo entre entrada e saída. A decisão é sua de onde fazer esse cálculo. Fique a vontade para incluir quaisquer métodos em qualquer uma das classes, desde que não quebre os testes já existentes, isto é, que não viole a interface original do objeto.

```java
// Spock é um Funcionário
// Ponto representa uma presença do funcionário
// Ponto é mutável
Ponto ponto = new Ponto("Spock");
// toString
System.out.println(ponto); // Spock não bateu ponto
// Spock bateu ponto às 7:50:15
ponto.bater(7, 50, 15);
System.out.println(ponto); // Spock entrou às 7:50:15
ponto.bater(12, 2, 10);
System.out.println(ponto); // Spock entrou às 7:50:15 e saiu às 12:02:10 e permaneceu 4 horas, 2 minutos e 10 segundos
System.out.println(ponto.toString().equals("Spock entrou às 7:50:15 e saiu às 12:02:10 e permaneceu 4 horas, 2 minutos e 10 segundos"));

Ponto ponto2 = new Ponto("Kirk");
ponto2.bater(14, 0, 0);
System.out.println(ponto2); // Kirk entrou às 14:00:00
ponto2.bater(3, 10, 0);
System.out.println(ponto2); // Kirk entrou às 14:00:00 e saiu às 03:10:00 e permaneceu 13 horas e 10 minutos
System.out.println(ponto2.toString().equals("Kirk entrou às 14:00:00 e saiu às 03:10:00 e permaneceu 13 horas e 10 minutos"));
// não deve ser possível bater o ponto fechado
try {
  ponto2.bater(4, 15, 8);
  System.out.println(false); // falhou
} catch (IllegalStateException e) {
  System.out.println(e.getMessage()); // Entrada e saída já foram batidas e o ponto está fechado
  System.out.println(true); // ok
}
```

* * *

> There are known knowns. These are things we know that we know.
> There are known unknowns. That is to say, there are things that we know we don't know.
> But there are also unknown unknowns. There are things we don't know we don't know.
>
> -- **Donald Rumsfeld**
