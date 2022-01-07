# DESIGN_PATTER_FIRMWARE
Documentação para definição de padrões de projetos de Firware para Smartek
#### Powered by jPerotto

## BASEADO NO PADRÃO MISRA-C.
## SUGESTÕES DE PROGRAMAÇÃO, CLEAN CODE.

## DOCUMENTAÇÃO DE CÓDIGO
* RDC-1 - Para documentação deve ser utilizado o padrão [DOXYGEN](https://www.doxygen.nl/index.html "Doxygen");
* RDC-2 - Para todo codigo ou biblioteca deve ser criado um header contendo um brief e author. Seja biblioteca ou aplicação;
* RDC-3 - Toda a documentação deve ser escrita em maiúscula quando não for padrão do próprio Doxygen, conforme vemos abaixo;
* RDC-4 - Todo comentário de código deve ser escrito sem qualquer acentuação.

```
/**
 * @brief DEMONSTRACAO DE PADRAO DE DOCUMENTACAO
 * @author jPerotto
 * @param E_MAIL_DE_CONTATO meu_e_mail_contato@provedor.dominio
 * @param E_MAIL_GITHUB meu_e_mail_github@provedor.dominio
 * @param varTeste VARIAVEL PARA FAZER X COISA
 * @return true SUCESSO false FALHOU
*/
```

### PADRÃO DE CÓDIGO
* RPC-1 - Abertura de chave deve ser feita na linha abaixo, para melhor identação;
* RPC-2 - Toda estrutura condicional deve ser separada por espaço de uma linha;
* RPC-3 - Formatação de código deve-se utilizar a padrão do VSCode (Padrão C++):
    * Ubuntu: Ctrl+ Shift+ I.
    * Windows: Shift+ Alt+ F.
    * Mac: Shift+ Option+ F.

### PADRÃO DE NOMEAÇÃO
* RPN-1 - A declaração deve iniciar com a responsabilidade, usabilidade e posteriormente com sua função:
    * Aplicável à, diretivas, variaveis, funções e demais usos da linguagem de progração C / C++.

### PADRÃO DE VARIÁVEIS
* RPV-1 - Os tipos de variáveis são explicitamente declaradas com seu formato e tamanho, para facilitar a portabilidade de arquitetura da aplicação, aumentando a segurança e padrão de código. Conforme a estrutura variaveis_t;
* RPV-2 - Todas as variáveis declaradas também devem ser inicializadas com um valor padrão, para evitar lixo e erros de operação;
* RPV-3 - Todas as variáveis devem ser declaradas utilizando padrão camelcase;
* RPV-4 - As variáveis devem segui o padrão de nomeação RN1;
* RPV-5 - Toda variável declarada para uso de um retorno de função, deve ser do mesmo tipo de retorno da função;
* RPV-6 - Agrupamento de variáveis com typedef struct, deve conter um nome com sufixo '_t'.

```
typedef struct
{
    //VARIAVEL INTEIRA BOOLEANA.
    bool var_b = false;

    //VARIAVEIS INTEIRAS DE 8 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int8_t var_i8 = NULL;           // -128 A 127
    uint8_t var_u8 = NULL;          // 0 A 255

    //VARIAVEIS INTEIRAS DE 16 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int16_t var_i16 = NULL;         // -32.768 A 32.767
    uint16_t var_u16 = NULL;        // 0 A 65535

    //VARIAVEIS INTEIRAS DE 32 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int32_t var_i32 = NULL;         // -2.147.483.648 A 2.147.483.647
    uint32_t var_u32 = NULL;        // 0 A 4.294.967.295

    //VARIAVEIS INTEIRAS DE 64 BITS, SIGNED E UNSIGNED RESPECTIVAMENTE.
    int64_t var_i64 = NULL;         // -9.223.372.036.854.775.808 A 9.223.372.036.854.775.807
    uint64_t var_u64 = NULL;        // 0 A 18.446.744.073.709.551.615

    //VETOR DE VARIAVEL CHAR.
    char SSID[SSID_MAX_LENGTH] = {NULL};
} variaveis_t;


uint32_t timeControlReadSensor = millis(); /** @see RPV-5 */
delay(TIME_DELAY); /** @see RULE RDC-1 */
if ((millis() - timeControlReadSensor) > TIME_NEW_READ)
{
    readSensorDefault();
}
```

### PADRÃO DE #DEFINE
* RPD-1 - Não utilizar números mágicos no código. Todos os valores constantes em código devem estar contidos em uma diretiva de pré-processamento (#define) para facilitar a manutenibilidade de código.
* RPD-2 - Diretivas devem ser declaradas tudo em letras maiúsculas, seguindo o padrao de definição de nomes RPN-1 e RDC-3.

```
#define USER_DEFAULT_WIFI_SMARTEK "SmartekLife" //USUARIO PADRAO DO WI-FI DA SMARTEK
```

### PADRÃO DE ENUM
* RPE-1 - Toda enum definida, deve ter um nome descrito seguindo o padrão RPN-1 e RDC-3;
* RPE-2 - Toda enum deve conte seus valores de forma explicita, mesmo que siga uma ordem conforme abaixo.

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
* RES-1 - Evitar ifs e estruturas de repetições com mais que 3 aninhamentos

#### PADRÕES DE IF
* RES-2 - Toda verificação de if, else if, deve contem um else. Porém quando o if é direto (onde o else não tem implicação de funcionamento), é opcional;
* RES-3 - Toda verificação de if com retorno de função booleana, ser verificado diretamente;
* RES-4 - Toda verificação de if que contém uma variável int8_t, uint8_t ou superior, deve conter o valor de veirificação em uma #define ou enum preferêncialmente;
* RES-5 - Toda verificação de if que contém mais que uma verificação, cada verificação deve estar separa por parenteses. Excessão (caso a verificação seja um booleano);
* RES-6 - Toda estrutura condicional, if, else if, else e switch devem conter chaves para separação de blocos, por mais que seja apenas uma linha de código.

#### PADRÕES DE SWITCH CASE
* RES-7 - Switch case deve conter blocos entre cada case conforme RES-6, obrigatóriamente o default e deve conter o break se for de execução única em cada case e também para o default.

#### PADRÕES DE FUNÇÕES
* RES-8 - Toda função sem parâmetro deve ser void, void funcao(void).
* RES-9 - Retorno de função deve existir em apenas um ponto;


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
