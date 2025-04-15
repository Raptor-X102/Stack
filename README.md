# Структура данных стек

## Краткое описание
Это моя реализация структуры данных стек с версиями debug и release.

## Debug версия

Включена по умолчанию, отключается флагом

```
-D NO_DEBUG
```

Имеется hash-защита, а также защита, использующая канарейки. Защищается структура и сам стек.

<details>
<summary>Структура стека</summary>

```c
struct Stack_t {

    #ifndef NO_CANARY_PROTECTION
    uint64_t left_struct_canary;
    #endif

    #ifndef NO_DEBUG
    const char* name;
    const char* file;
    const char* func;
    int line;
    #endif

    void * data;
    size_t element_size;
    const void * poison_val;
    char* free_data;
    int64_t size;
    int64_t capacity;
    bool Stack_Status;
    size_t mem_size_aligned;

    #ifndef NO_HASH_PROTECTION
    size_t struct_hash_sum;
    size_t data_hash_sum;
    #endif

    #ifndef NO_CANARY_PROTECTION
    uint64_t right_struct_canary;
    #endif
};
```
</details>

Стэк может хранить любой тип и размер данных. В Debug версии также нужно передать poizon-значение, которым заполняется все неиспользованное пространство.

Иеется подробный вывод в файл о состоянии стека (StackDump)

<details>
<summary>Пример StackDump</summary>

```
Stack_t [007AFE98]
    called from Stack_test.cpp:57 (main)
    name "&stack" born at Stack_test.cpp:13 (main) {
        left_struct_canary = 0xd15ab1ed
        right_struct_canary = 0x1dea
        capacity = 16
        size = 10
        mem_size_aligned = 144
        struct_hash_sum = -519019465
        data_hash_sum = 426095118
        data = [011D1728] {   * - element belongs to stack
            left_data_canary (011D1720) = 0xb1e55ed
            *[0] = 10
            *[1] = 20
            *[2] = 30
            *[3] = 40
            *[4] = 50
            *[5] = 60
            *[6] = 80
            *[7] = 90
            *[8] = 100
            *[9] = 110
             [10] = -3.63536e-132
             [11] = -3.63536e-132
             [12] = -3.63536e-132
             [13] = -3.63536e-132
             [14] = -3.63536e-132
             [15] = -3.63536e-132
            right_data_canary (011D17A8) = 0xc0de
        }
    }
```
</details>

## Функционал
| Функция     	| Описание                                                   	|
|-------------	|---------------------------------------------------------------|
| StackCtor   	| Создание стека                                             	|
| StackPush   	| Положить в стек элемент                                    	|
| StackPop    	| Достать элемент из стека                                   	|
| StackResize 	| Увеличить или уменьшить стек<br>(недоступна пользователю) 	|
| StackDtor   	| Уничтожить стек                                            	|
| Stack_Dump  	| Функция вывода в dump                                      	|
