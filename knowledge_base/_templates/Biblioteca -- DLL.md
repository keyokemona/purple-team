---
tipo: biblioteca
subtipo: <nativa-windows | runtime-vc | third-party>
caminho_padrao: "C:\\Windows\\System32"
risco_hijacking: <baixo | medio | alto>
aliases:
  - <NomeDaDLLSemExtensao>
  - <nome_da_dll.dll>
---

# <Nome da DLL/Biblioteca> (<Nome por extenso / Significado>)

> **O que é:** <Definição concisa e direta do componente (ex.: biblioteca principal de suporte em tempo de execução do subsistema Win32, biblioteca padrão C++, runtime CRT, etc.).>

---

## 1. Para que serve (Atribuições & Responsabilidades no SO)

* **<Atribuição / Funcionalidade 1>:** <Explicação objetiva do papel da DLL em tempo de execução, ex.: gerenciamento de memória virtual e heaps>
* **<Atribuição / Funcionalidade 2>:** <Ex.: criação e escalonamento de threads/processos>
* **<Atribuição / Funcionalidade 3>:** <Ex.: intermediação de chamadas para o kernel via ntdll.dll>

---

## 2. Distribuição & Origem

* **Localização Padrão:** `C:\Windows\System32\` (x64) e `C:\Windows\SysWOW64\` (x86) / `<caminho_alternativo>`
* **Mecanismo de Entrega:** <Componente nativo do Windows OS | Instalado via Microsoft Visual C++ Redistributable (vcredist) | Componente proprietário de terceiros>
* **Proteções de Carregamento:** <Ex.: Listada na chave de registro KnownDLLs (imune a DLL Hijacking em caminhos padrão) ou vulnerável a DLL Search Order Hijacking>

---

## 3. Funções Notáveis Frequentemente Vistas em Malwares

### Injeção de Código & Manipulação de Memória
* `[[<NomeDaFuncao1>]]` - <Breve resumo do propósito em malwares>
* `[[<NomeDaFuncao2>]]` - <Breve resumo do propósito em malwares>

### Evasão & Anti-Análise (Anti-Debug / Anti-VM)
* `[[<NomeDaFuncao3>]]` - <Breve resumo do propósito em malwares>

### Comunicação de Rede & Exfiltração (se aplicável)
* `[[<NomeDaFuncao4>]]` - <Breve resumo do propósito em malwares>

---

## 4. Relevância em Triage Estática (IAT) & Purple Teaming

* **Leitura da IAT:** <O que a presença desta DLL indica em uma triagem preliminar? Binário legítimo com muitas APIs, ou binário pequeno com poucos imports característico de um packer/stager?>
* **Vetores de Abuso (Red Team / Defesa):**
  * <Ex.: DLL Sideloading (carregada por executável legítimo assinado), DLL Search Order Hijacking, Phantom DLL Hijacking>

---

## 5. Referências & Amostras Analisadas

* [Microsoft Learn: Documentação Oficial de Bibliotecas e DLLs](https://learn.microsoft.com/)
* **Amostras no Vault que utilizam esta DLL:**
  * [[<Amostra-ou-Lab-01>]]
