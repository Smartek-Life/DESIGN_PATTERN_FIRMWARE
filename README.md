# DESIGN_PATTER_FIRMWARE
Documentação para definição de padrões de projetos de Firware para Smartek
#### Powered by jPerotto

## BASEADO NO PADRÃO MISRA-C.
## SUGESTÕES DE PROGRAMAÇÃO, CLEAN CODE.

## DOCUMENTAÇÃO DE CÓDIGO
* RD1 - Para documentação deve ser utilizado o padrão [DOXYGEN](https://www.doxygen.nl/index.html "Doxygen").
* RD2 - Toda a documentação deve ser escrita em maiúscula quando não for padrão do próprio Doxygen, conforme vemos abaixo:
* RD3 - Todo comentário de código deve ser escrito sem qualquer acentuação.

```
/**
 * @brief DEMONSTRACAO DE PADRAO DE DOCUMENTACAO
 * @author jPerotto
 * @param varTeste VARIAVEL PARA FAZER X COISA
 * @return true SUCESSO false FALHOU
*/
```

### PADRÃO DE DESENVOLVIMENTO
* RD1 - Desenvolver utilizando apenas o core do ESP8266 da smartek com as boards config.
* RD2 - Para lançamento de release, deve ser testado o OTA depois do OTA.

### PADRÃO DE CÓDIGO
* RC1 - Abertura de chave deve ser feita na linha abaixo, para melhor identação.
* RC2 - Formatação de código deve-se utilizar a padrão do VSCode (Padrão C++).
    * Ubuntu: Ctrl+ Shift+ I.
    * Windows: Shift+ Alt+ F.
    * Mac: Shift+ Option+ F.

### PADRÃO DE NOMEAÇÃO
* RN1 - A declaração deve iniciar com a responsabilidade, usabilidade e posteriormente com sua função. Conforme Padrões abaixo

### PADRÃO DE VARIÁVEIS
* RV1 - Os tipos de variáveis são explicitamente declaradas para facilitar a portabilidade de arquitetura da aplicação, aumentar a segurança e padrão de código.
* RV2 - Todas as variáveis declaradas também devem ser inicializadas com um valor padrão, para evitar lixo e erros de operação.
* RV3 - Todas as variáveis devem ser declaradas utilizando padrão camelcase.
* RV4 - As variáveis devem segui o padrão de nomeação RN1.
* RV5 - Variáveis de controle temporal devem ser declaradas como uint32_t e comparadas posterior ao método de tempo para garantir rollout.
* RV6 - Agrupamento de variáveis com typedef struct, deve conter um nome com sufixo '_t'

```
typedef struct
{
    //VARIAVEL INTEIRA BOOLEANA.
    bool sensorAtivado = false;

    //VARIAVEIS INTEIRAS DE 8 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int8_t sensorTemperatura = NULL;    // -128 A 127
    uint8_t sensroUmidade = NULL;       // 0 A 255

    //VARIAVEIS INTEIRAS DE 16 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int16_t sensorTemperatura = NULL;   // -32.768 A 32.767
    int16_t sensorUmidade = NULL;       // 0 A 65535

    //VARIAVEIS INTEIRAS DE 32 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int32_t sensorTemperatura = NULL;   // -2.147.483.648 A 2.147.483.647
    int32_t sensorUmidade = NULL;       // 0 A 4.294.967.295

    //VARIAVEIS INTEIRAS DE 64 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int64_t sensorTemperatura = NULL;   // -9.223.372.036.854.775.808 A 9.223.372.036.854.775.807
    int64_t sensorUmidade = NULL;       // 0 A 18.446.744.073.709.551.615

    //VETOR DE VARIAVEL CHAR.
    char SSID[SSID_MAX_LENGTH] = {NULL};
} variaveis_t;


uint32_t timeControlReadSensor = millis(); /** @see RV5 */
delay(TIME_DELAY); /** @see RULE RD-1 */
if ((millis() - timeControlReadSensor) > TIME_NEW_READ)
{
    readSensorDefault();
}
```

### PADRÃO DE #DEFINE
* RD1 - Não utilizar números mágicos no código. Todos os valores constantes em código devem estar contidos em uma diretiva de pré-processamento (#define) para facilitar a manutenibilidade de código.
* RD2 - Diretivas devem ser declaradas tudo em letras maiúsculas, seguindo o padrao de definição de nomes RN1.

```
#define USER_WIFI_SMARTEK //USUARIO PADRAO DO WI-FI DA SMARTEK
#define 
```

### PADRÃO DE ENUM
* RE1 - Toda enum definida, deve ter um nome descrito seguindo o padrão RD2.
* RE2 - Toda enum deve conte seus valores de forma explicita.

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
* RS1 - Evitar ifs e estruturas de repetições com mais que 3 aninhamentos

#### PADRÕES DE IF
* RS2 - Toda verificação de if, else if, deve contem um else. Porém quando o if é direto, é opcional.
* RS3 - Toda verificação de if com retorno de função booleana, variável booleana deve ser verificado diretamente.
* RS4 - Toda verificação de if que contem uma variável int8_t, uint8_t ou superior, deve conter o valor de veirificação em uma #define ou enum
* RS5 - Toda verificação de if que contem mais que uma verificação, cada verificação deve estar separa por parenteses. Excessão (caso a verificação seja um booleano).
* RS6 - Todo if deve conter bloco, por mais que seja apenas uma linha de código.

#### PADRÕES DE SWITCH CASE
* RS7 - Switch case deve conter blocos entre cada case, e o default deve conter o break.

#### PADRÕES DE FUNÇÕES
* RS8 - Toda função sem parâmetro deve ser void, void fnc(void).
* RS9 - Retorno de função deve existir em apenas um ponto.


```
#define SENSOR_TEMPERATURA 2
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

void main(void)
{
    if (!sensorRead)
    {
        valorSensorTemperatura = readSensor(SENSOR_TEMPERATURA);
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
}


uint8_t readSensor(uint8_t numberSensor)
{
    switch(numberSensor)
    {
        case 1:
        {
            readSensor1();
            break;
        }
        case 2:
        {
            readSensor2();
            break;
        }
        default:
        {
            readSensorDefault();
            break;
        }
    }
}

```
