# Ingeniería del software I 

# Reto 001 - Modelado: Farmear aura

## Diagrama de clases

```mermaid
classDiagram
    class Persona {
        nombre
        aura() Integer
        esFarmeador() Boolean
    }
    class Movida {
        queHizo
        hora
        aProposito Boolean
        auraDeLaMovida() Integer
    }
    class Reaccion {
        puntos Integer
        seLoCreyoNatural Boolean
    }
    class Escena {
        donde
        queEstabaPasando
        queTanHeavy Integer
    }
    class Historial {
        fecha
        auraQueTenia Integer
    }
    class Tipo {
        <<enumeration>>
        W
        FRIALDAD
        ESTILO
        CRINGE
    }

    Persona "1" --> "*" Movida : hace
    Persona "1" --> "*" Reaccion : reacciona como publico
    Movida "1" --> "*" Reaccion : recibe
    Movida "*" --> "1" Escena : pasa en
    Movida "*" --> "1" Tipo : es de tipo
    Persona "1" --> "*" Historial : guarda

    note for Reaccion "Los puntos pueden ser negativos: el aura se gana y se PIERDE. Aura de alguien = suma de las reacciones a sus movidas."
    note for Movida "Si nadie te vio, vale 0. Si la hiciste a proposito y se nota que farmeas, te resta aura (cringe)."
```

## ¿Qué pasa con una movida?

```mermaid
stateDiagram-v2
    state "Nadie la vio" as NadieLaVio
    state "Suma aura (W)" as SumaAura
    state "Resta aura (cringe)" as RestaAura

    [*] --> Hecha
    Hecha --> NadieLaVio : sin publico
    NadieLaVio --> [*] : vale 0
    Hecha --> Vista : alguien la vio
    Vista --> Reaccionada : el publico reacciona
    Reaccionada --> Reaccionada : reacciona mas gente
    Reaccionada --> SumaAura : total > 0
    Reaccionada --> RestaAura : total <= 0
    SumaAura --> [*]
    RestaAura --> [*]
```

## Ejemplo: Ana vs Luis

```mermaid
flowchart LR
    Ana([Ana]) -->|hace| M1["Camina sin mirar la explosion<br/>a proposito: NO"]
    Luis([Luis]) -->|hace| M2["Posa 10 min para que lo graben<br/>a proposito: SI"]
    Eva([Eva - publico]) -->|reacciona| R1["+50<br/>se lo creyo natural"]
    Eva -->|reacciona| R2["-20<br/>se nota que farmea"]
    R1 --> M1
    R2 --> M2
    M1 --- T1{{FRIALDAD}}
    M2 --- T2{{CRINGE}}
```

## Glosario

| Término | Significa |
|---|---|
| **Aura** | Puntos de "presencia" que tienes. Pueden ser negativos. |
| **Farmear** | Hacer movidas a propósito para sacar aura. |
| **Movida** | Cosa que haces y que otros pueden ver. |
| **Reacción** | Lo que opina, en puntos, alguien que te vio. |
| **Escena** | El momento y lugar donde pasa la movida. |
| **W** | Movida que suma aura. |
| **Cringe** | Movida que resta aura. |

## Supuestos

1. Sin público no hay aura.
2. El aura no se guarda: se calcula sumando reacciones.
3. Puedes perder aura (quedarte en negativo).
4. No cuenta reaccionar a tus propias movidas.
5. Cada persona del público pone sus propios puntos.

## Decisiones discutibles

- **"A propósito" es un dato de la movida**, porque farmear es hacerlo con intención. Pero lo que cuenta es si el público se lo cree (`seLoCreyoNatural`): farmear de forma obvia da cringe.
- **La Escena va aparte**: la misma movida vale distinto según lo heavy que sea el momento (ignorar una explosión no es lo mismo que ignorar un mosquito).
- **Tipo es una lista** (W, frialdad, estilo, cringe) y no clases separadas, porque solo sirve para clasificar.
- **Reacción es una clase**, no una simple relación, porque tiene datos propios (puntos y si se lo creyó natural).
- **Quedan fuera**: redes sociales, que el aura se apague con el tiempo y el aura de grupos.
