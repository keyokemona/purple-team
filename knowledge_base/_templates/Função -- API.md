---
tipo: api-funcao
categoria: <categoria_ex_injection_evasion_persistence_network_discovery>
biblioteca: "[[<nome_da_dll_ex_kernel32.dll>]]"
header: "<header_ex_windows.h>"
mitre_attack:
  - "[[<tecnica_ex_T1055>]]"
status: rascunho # rascunho | revisado
aliases:
  - <NomeDaFuncao>
---

# <Nome da Função / API> (<Header / Biblioteca associada>)

> **O que é:** <Descrição direta e concisa da função, definindo seu propósito central no sistema operacional.>

---

## 1. Assinatura & Requisitos

* **Header:** `<header.h>` (ex.: `<memoryapi.h>`, `<windows.h>`, `<process.h>`)
* **DLL de Exportação:** `<modulo.dll>` (ex.: `kernel32.dll`, `ntdll.dll`, `ucrtbase.dll`)
* **Privilégios / Contexto:** <Restrições de privilégio, ex.: requer SeDebugPrivilege, integridade alta, ou roda em user-mode padrão>
* **Retorno & Tratamento de Erros:** <Valor retornado em caso de sucesso vs falha. Ex.: Retorna ponteiro/handle em sucesso, NULL em erro; detalhes obtidos via GetLastError() ou código NTSTATUS/HRESULT>

```cpp
<TIPO_RETORNO> <NomeDaFuncao>(
  [in]           <TIPO> <param1>,
  [in, optional] <TIPO> <param2>,
  [out]          <TIPO> <param3>
);
```

---

## 2. Parâmetros Críticos na Análise (O que observar no Debugger / Decompiler)

* `<parametro_alvo_1>`: <Explicação do porquê esse argumento é crítico em triagem. Ex.: handle de processo de terceiros, flags suspeitas, alocação RWX (0x40)>
* `<parametro_alvo_2>`: <Significado de valores constantes, buffers de entrada/saída ou tamanhos alocados>

---

## 3. Relevância em Malware Analysis (Uso Malicioso & IAT)

* **Para que serve (Uso Malicioso):**
  * <Tópico 1: Cenário comum de abuso, ex.: alocação de payload em processo remoto>
  * <Tópico 2: Encadeamento típico com outras APIs, ex.: [[API_Anterior]] -> [[Esta_API]] -> [[Proxima_API]]>
* **Associação a Regras do Mandiant Capa:**
  * `<namespace/regra-do-capa>` (ex.: `host-interaction/process/inject`, `anti-analysis/anti-debugging`)
* **Significado na IAT (Triagem Estática):**
  * <O que a presença desta função na tabela de importação nos diz antes mesmo de rodar a amostra>

---

## 4. Telemetria & Detecção (Purple Teaming)

* **Fontes de Eventos:**
  * <Sysmon (ex.: Event ID 8 - CreateRemoteThread, Event ID 10 - ProcessAccess), Telemetria de EDR / User-mode hooking, Logs de Auditoria do Windows>
* **Heurística de Detecção & Hunting:**
  * <Critérios para alertar, ex.: chamada originada de memória não mapeada (unbacked memory) ou alocação RWX em executáveis como explorer.exe / svchost.exe>

---

## 5. Referências & Amostras Analisadas

* [Microsoft Learn: Documentação Oficial da Função](https://learn.microsoft.com/)
* **Amostras no Vault:**
  * [[<Amostra-ou-Lab-01>]]
