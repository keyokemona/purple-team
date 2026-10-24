# Descrição e utilização
O *iat-eat-parser* é uma ferramenta escrita em Python (com a ajuda do Gemini) pra obter as bibliotecas, importações e exportações de um binário (apontado pelo seu filepath completo), e trazer um agregado das informações de forma simplificada num arquivo .json, assimilando diretamente as funções listadas por cada biblioteca/função da IAT (Import Address Table) e/ou EAT (Export Address Table).

Exemplo de utilização:
`iat-eat-parser .\Malware.Unknown.exe.malz`

\[C:\\Users\\keyokemona\\Desktop\\Malware.Unknown.exe.malz\]
```json
{
    "arquivo_analisado": "C:\\Users\\keyokemona\\Desktop\\Malware.Unknown.exe.malz",
    "importacoes_IAT": {
        "KERNEL32.dll": [
            "GetModuleFileNameW",
            "CloseHandle",
            "CreateProcessW",
            "GetCurrentProcessId",
            "GetCurrentThreadId",
            "IsProcessorFeaturePresent",
            "GetSystemTimeAsFileTime",
            "InitializeSListHead",
            "IsDebuggerPresent",
            "TerminateProcess",
            "GetCurrentProcess",
            "GetModuleHandleW",
            "UnhandledExceptionFilter",
            "SetUnhandledExceptionFilter",
            "QueryPerformanceCounter"
        ],
        "SHELL32.dll": [
            "ShellExecuteW"
        ],
        "MSVCP140.dll": [
            "_Query_perf_frequency",
            "_Thrd_sleep",
            "_Query_perf_counter",
            "_Xtime_get_ticks"
        ],
        "urlmon.dll": [
            "URLDownloadToFileW"
        ],
        "WININET.dll": [
            "InternetOpenW",
            "InternetOpenUrlW"
        ],
        "VCRUNTIME140.dll": [
            "__current_exception",
            "__current_exception_context",
            "_except_handler4_common",
            "memset"
        ],
        "api-ms-win-crt-stdio-l1-1-0.dll": [
            "__p__commode",
            "__stdio_common_vswprintf",
            "_set_fmode"
        ],
        "api-ms-win-crt-runtime-l1-1-0.dll": [
            "_c_exit",
            "_seh_filter_exe",
            "_initialize_onexit_table",
            "_register_onexit_function",
            "_crt_atexit",
            "_controlfp_s",
            "terminate",
            "_set_app_type",
            "_configure_narrow_argv",
            "_register_thread_local_exe_atexit_callback",
            "_cexit",
            "__p___argv",
            "__p___argc",
            "_exit",
            "exit",
            "_initterm_e",
            "_initterm",
            "_get_initial_narrow_environment",
            "_initialize_narrow_environment"
        ],
        "api-ms-win-crt-math-l1-1-0.dll": [
            "__setusermatherr"
        ],
        "api-ms-win-crt-locale-l1-1-0.dll": [
            "_configthreadlocale"
        ],
        "api-ms-win-crt-heap-l1-1-0.dll": [
            "_set_new_mode"
        ]
    },
    "exportacoes_EAT": []
}
```

# Código fonte
Pré-requisitos (apesar da maioria - se não todos - serem bulit-in do Python):
- os
- sys
- json
- argparse
- pefile
- pyinstaller

Código:
```python
import os
import sys
import json
import argparse
import pefile

def gerar_json_pe(caminho_artefato):
    try:
        # Garante o caminho absoluto do artefato
        caminho_abs = os.path.abspath(caminho_artefato)
        if not os.path.exists(caminho_abs):
            print(f"[-] Erro: O arquivo '{caminho_artefato}' não foi encontrado.")
            sys.exit(1)

        # Define o nome do arquivo JSON de saída no diretório atual de trabalho
        nome_base = os.path.splitext(os.path.basename(caminho_abs))[0]
        caminho_saida = os.path.join(os.getcwd(), f"{nome_base}_pe_report.json")

        # Carrega o binário utilizando o pefile
        pe = pefile.PE(caminho_abs)
        
        # Estrutura base do JSON
        dados_pe = {
            "arquivo_analisado": caminho_abs,
            "importacoes_IAT": {},
            "exportacoes_EAT": []
        }
        
        # 1. Mapeando a IAT
        if hasattr(pe, 'DIRECTORY_ENTRY_IMPORT'):
            for entrada in pe.DIRECTORY_ENTRY_IMPORT:
                dll_nome = entrada.dll.decode('utf-8', errors='ignore')
                dados_pe["importacoes_IAT"][dll_nome] = []
                
                for func in entrada.imports:
                    nome_func = func.name.decode('utf-8', errors='ignore') if func.name else f"Ordinal_{func.ordinal}"
                    dados_pe["importacoes_IAT"][dll_nome].append(nome_func)
        
        # 2. Mapeando a EAT
        if hasattr(pe, 'DIRECTORY_ENTRY_EXPORT'):
            for exp in pe.DIRECTORY_ENTRY_EXPORT.symbols:
                nome_func = exp.name.decode('utf-8', errors='ignore') if exp.name else f"Ordinal_{exp.ordinal}"
                dados_pe["exportacoes_EAT"].append({
                    "funcao": nome_func,
                    "ordinal": exp.ordinal,
                    "rva": hex(exp.address)
                })
                
        # Salvando o resultado estruturado em formato JSON no diretório de execução
        with open(caminho_saida, 'w', encoding='utf-8') as f:
            json.dump(dados_pe, f, indent=4, ensure_ascii=False)
            
        print(f"[+] Relatório JSON gerado com sucesso em: {caminho_saida}")
        
    except Exception as e:
        print(f"[-] Erro ao processar o artefato: {e}")
        sys.exit(1)

if __name__ == "__main__":
    parser = argparse.ArgumentParser(description="Extrai IAT e EAT de um artefato PE para um arquivo JSON estruturado.")
    parser.add_argument("filepath", help="Caminho completo ou relativo do artefato PE (EXE, DLL, SYS, etc.)")
    
    args = parser.parse_args()
    gerar_json_pe(args.filepath)

```

Comando para compilação:
`pyinstaller --onefile --console iat-eat-parser.py