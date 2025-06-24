
# 🎯 Padrões de Projeto: Singleton e Factory Method - Murilo Noguêz, Silvio Castilhos

# 🔷 Singleton

## ✔️ Definição

O padrão **Singleton** garante que uma classe tenha **apenas uma instância** e fornece um **ponto de acesso global** a ela.

---

## 💡 Quando Usar?

- Quando deve haver **uma única instância** de uma classe no sistema;
- Quando precisa de **acesso global** a um recurso compartilhado (ex: Logger, Configuração, Conexão com Banco de Dados).

---

## 🚫 Exemplo **Sem** Singleton (TypeScript)

```ts
// loggerSemSingleton.ts

class Logger {
  log(message: string): void {
    console.log(`[LOG]: ${message}`);
  }
}

const logger1 = new Logger();
const logger2 = new Logger();

logger1.log("Primeira instância");
logger2.log("Segunda instância");

console.log(logger1 === logger2); // false
```

---

## ✅ Exemplo **Com** Singleton (TypeScript)

```ts
// loggerComSingleton.ts

class SingletonLogger {
  private static instance: SingletonLogger;

  private constructor() {}

  static getInstance(): SingletonLogger {
    if (!SingletonLogger.instance) {
      SingletonLogger.instance = new SingletonLogger();
    }
    return SingletonLogger.instance;
  }

  log(message: string): void {
    console.log(`[LOG]: ${message}`);
  }
}

const loggerA = SingletonLogger.getInstance();
const loggerB = SingletonLogger.getInstance();

loggerA.log("Usando instância A");
loggerB.log("Usando instância B");

console.log(loggerA === loggerB); // true
```

---

## ⚖️ Pontos Fortes

✅ Garante apenas uma instância  
✅ Ideal para recursos globais  
✅ Simples de implementar

---

## ❌ Pontos Fracos

❌ Dificulta testes (difícil de "mockar")  
❌ Pode violar SRP (Responsabilidade Única)  
❌ Pode causar acoplamento global

---

# 🏭 Factory Method

## 🔷 O que é?

O **Factory Method** é um padrão **criacional** que fornece uma interface para criar objetos, mas permite que **subclasses decidam qual classe concreta instanciar**.

---

## 💡 Quando Usar?

- Para **desacoplar** o código cliente da criação dos objetos;
- Quando há **múltiplos tipos** que compartilham a mesma interface;
- Quando o sistema precisa ser **escalável e extensível**.

---

## 🚫 Exemplo Sem Factory Method

```ts
class EmailNotificacao {
  enviar(msg: string) {
    console.log(`[Email] ${msg}`);
  }
}

class SMSNotificacao {
  enviar(msg: string) {
    console.log(`[SMS] ${msg}`);
  }
}

const tipo = "sms";
let notificacao;

if (tipo === "email") {
  notificacao = new EmailNotificacao();
} else if (tipo === "sms") {
  notificacao = new SMSNotificacao();
}

notificacao.enviar("Você tem uma nova mensagem.");
```

---

## ✅ Exemplo Com Factory Method

```ts
interface Notificacao {
  enviar(msg: string): void;
}

class EmailNotificacao implements Notificacao {
  enviar(msg: string) {
    console.log(`[Email] ${msg}`);
  }
}

class SMSNotificacao implements Notificacao {
  enviar(msg: string) {
    console.log(`[SMS] ${msg}`);
  }
}

abstract class NotificacaoFactory {
  abstract criar(): Notificacao;
}

class EmailFactory extends NotificacaoFactory {
  criar(): Notificacao {
    return new EmailNotificacao();
  }
}

class SMSFactory extends NotificacaoFactory {
  criar(): Notificacao {
    return new SMSNotificacao();
  }
}

const factory: NotificacaoFactory = new SMSFactory();
const notificacao = factory.criar();
notificacao.enviar("Você tem uma nova mensagem.");
```

---

## 🟢 Benefícios

✔️ Código limpo, modular e extensível  
✔️ Fácil adicionar novos tipos (ex: WhatsApp, Push)  
✔️ Reduz o acoplamento

---

## ❌ Pontos Fracos

✖️ Mais classes e abstrações  
✖️ Pode ser exagerado em sistemas simples

---

# 🧠 Comparativo: Singleton vs Factory Method

| Aspecto                     | Singleton                                            | Factory Method                                                        |
|----------------------------|------------------------------------------------------|------------------------------------------------------------------------|
| 🎯 Objetivo                | Uma instância global                                 | Delegar criação de instâncias                                         |
| 🧱 Categoria                | Criacional                                           | Criacional                                                             |
| 🧠 Controle de Instância    | Sim (única)                                          | Não                                                                   |
| 🔄 Flexibilidade           | Baixa                                                | Alta                                                                  |
| 🔌 Acoplamento             | Alto (instância global)                              | Baixo (interface + fábrica)                                           |
| 🧪 Testabilidade           | Baixa (mock é difícil)                               | Alta (mock fácil via interface)                                       |
| 📦 Exemplos de uso         | Logger, Configuração, Cache                          | Sistema de notificações, fábrica de documentos                        |
| 🧩 Complexidade estrutural | Baixa                                                | Média a alta                                                          |

---

## ✅ Conclusão do Comparativo

- **Singleton** é ótimo para garantir **uma única instância** global de um recurso.
- **Factory Method** é ideal quando o sistema precisa de **flexibilidade para criar objetos sem depender diretamente das classes concretas**.

👉 Use Singleton quando você quer **controle absoluto sobre instâncias**.  
👉 Use Factory Method quando você quer **desacoplar criação de uso**, facilitando a expansão do sistema.
