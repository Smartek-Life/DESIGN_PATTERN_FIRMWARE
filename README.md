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

### PADRÃO DE VARIÁVEIS
RV1 - Os tipos de variáveis são explicitamente declaradas para facilitar a portabilidade de arquitetura da aplicação, aumentar a segurança e padrão de código.
RV2 - Todas as variáveis declaradas também devem ser inicializadas com um valor padrão, para evitar lixo e erros de operação.
RV3 - Todas as variáveis devem ser declaradas utilizando padrão camelcase.
RV4 - A declaração deve iniciar com a responsabilidade da variável e posteriormente com sua função.

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

### PADRÃO DE DECLARAÇÃO
RD1 - Não utilizar números mágicos no código. Todos os valores constantes em código devem estar contidos em uma diretiva de pré-processamento (#define) para facilitar a manutenibilidade de código.