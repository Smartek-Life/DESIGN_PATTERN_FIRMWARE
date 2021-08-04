# DESIGN_PATTER_FIRMWARE
Documentação para definição de padrões de projetos de Firware para Smartek

## powered by jPerotto

## BASEADO NO PADRÃO MISRA-C.
## SUGESTÕES DE PROGRAMAÇÃO, CLEAN CODE.

## DOCUMENTAÇÃO DE CÓDIGO
Para documentação deve ser utilizado o padrão [DOXYGEN](https://www.doxygen.nl/index.html "Doxygen")
Toda a documentação deve ser escrita em maiúscula quando não for padrão do próprio Doxygen, conforme vemos abaixo:

```
/**
 * @brief DEMONSTRACAO DE PADRAO DE DOCUMENTACAO
 * @author jPerotto
 * @param varTeste VARIAVEL PARA FAZER X COISA
 * @return true SUCESSO false FALHOU
*/
```

### PADRÃO DE NOMEAÇÃO
RN1 - A declaração deve iniciar com a responsabilidade, usabilidade e posteriormente com sua função. Conforme Padrões abaixo

### PADRÃO DE VARIÁVEIS
RV1 - Os tipos de variáveis são explicitamente declaradas para facilitar a portabilidade de arquitetura da aplicação, aumentar a segurança e padrão de código.
RV2 - Todas as variáveis declaradas também devem ser inicializadas com um valor padrão, para evitar lixo e erros de operação.
RV3 - Todas as variáveis devem ser declaradas utilizando padrão camelcase.
RV4 - As variáveis devem segui o padrão de nomeação RN1.

```
//VARIAVEL INTEIRA BOOLEANA.
bool sensorAtivado = false;

//VARIAVEIS INTEIRAS DE 8 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
int8_t sensorTemperatura = NULL;
uint8_t sensroUmidade = NULL;

//VARIAVEIS INTEIRAS DE 16 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
int16_t sensorTemperatura = NULL;
int16_t sensorUmidade = NULL;

//VARIAVEIS INTEIRAS DE 32 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
int32_t sensorTemperatura = NULL;
int32_t sensorUmidade = NULL;

//VARIAVEIS INTEIRAS DE 64 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
int64_t sensorTemperatura = NULL;
int64_t sensorUmidade = NULL;
```

RV5 - Variáveis de controle temporal devem ser declaradas como uint32_t e comparadas posterior ao método de tempo para garantir rollout.
```
uint32_t timeSensorRead = millis();
delay(TIME_DELAY); /** @see RULE RD-1 */
if ((millis() - timeSensorRead) > TIME_NEW_READ)
{
    readSensor();
}
```

RV6 - Varia

### PADRÃO DE #DEFINE
RD1 - Não utilizar números mágicos no código. Todos os valores constantes em código devem estar contidos em uma diretiva de pré-processamento (#define) para facilitar a manutenibilidade de código.
RD2 - Diretivas devem ser declaradas tudo em letras maiúsculas, seguindo o padrao de definição de nomes RN1.

```
#define USER_WIFI_SMARTEK //USUARIO PADRAO DO WI-FI DA SMARTEK
#define 
```

### PADRÃO DE ENUM
RE1 - Toda enum definida, deve ter um nome descrito seguindo o padrão RD2.
RE2 - Toda enum deve conte seus valores de forma explicita.

```
enum ESCALA_SENSOR
{
    OUT_OF_SCALE = 0,
    ESCALE_1 = 1,
    ESCALE_2 = 2,
    ERRO_OF_SCALE = 3
}
```


### PADRÃO DE ENGENHARIA DE SOFTWARE
RS1 - Toda verificação de if, else if, deve contem um else. Porém quando o if é direto, é opcional.
RS2 - Toda verificação de if com retorno de função booleana, variável booleana deve ser verificado diretamente.
RS3 - Toda verificação de if que contem uma variável int8_t, uint8_t ou superior, deve conter o valor de veirificação em uma #define ou enum
RS4 - Toda verificação de if que contem mais que uma verificação, cada verificação deve estar separa por parenteses. Excessão (caso a verificação seja um booleano)

```
#define MIN_VALOR_SENSOR 10
#define MAX_VALOR_SENSOR 100
bool sensorReadRTemperatura = NULL;
uint8_t escalaTemperaturaSensor = NULL;
uint8_t valorSensorTemperatura = NULL;

enum ESCALA_SENSOR
{
    OUT_OF_SCALE = 0,
    ESCALE_1 = 1,
    ESCALE_2 = 2,
    ERRO_OF_SCALE = 3
}

if (!sensorRead)
{
    valorSensorTemperatura = readSensor();
    sensorRead = false;
}

if (!valorSensorTemperatura || (valorSensorTemperatura < MIN_VALOR_SENSOR))
{
    escalaTemperaturaSensor = OUT_OF_SCALE;
}
else if ((valorSensorTemperatura < MAX_VALOR_SENSOR) && (valorSensorTemperatura >= MIN_VALOR_SENSOR))
{
    escalaTemperaturaSensor = ESCALA_1;
}
else if(valorSensorTemperatura > MAX_VALOR_SENSOR)
{
    escalaTemperaturaSensor = ESCALA_2;
}
else
{
    escalaTemperaturaSensor = ERRO_OF_SCALE;
}

```