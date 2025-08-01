<!--
CO_OP_TRANSLATOR_METADATA:
{
  "original_hash": "88258b03f2893aa2e69eb8fb24baabbc",
  "translation_date": "2025-07-12T09:33:07+00:00",
  "source_file": "04-tool-use/README.md",
  "language_code": "pl"
}
-->
[![Jak projektować dobre agentów AI](../../../translated_images/lesson-4-thumbnail.546162853cb3daffd64edd92014f274103f76360dfb39fc6e6ee399494da38fd.pl.png)](https://youtu.be/vieRiPRx-gI?si=cEZ8ApnT6Sus9rhn)

> _(Kliknij powyższy obraz, aby obejrzeć wideo z tej lekcji)_

# Wzorzec projektowy użycia narzędzi

Narzędzia są interesujące, ponieważ pozwalają agentom AI na szerszy zakres możliwości. Zamiast ograniczonego zestawu działań, które agent może wykonać, dodanie narzędzia umożliwia agentowi realizację wielu różnych zadań. W tym rozdziale przyjrzymy się Wzorcu projektowemu użycia narzędzi, który opisuje, jak agenci AI mogą korzystać ze specyficznych narzędzi, aby osiągać swoje cele.

## Wprowadzenie

W tej lekcji postaramy się odpowiedzieć na następujące pytania:

- Czym jest wzorzec projektowy użycia narzędzi?
- Do jakich zastosowań można go stosować?
- Jakie elementy/bloki konstrukcyjne są potrzebne do implementacji tego wzorca?
- Jakie są szczególne kwestie do rozważenia przy używaniu wzorca użycia narzędzi do budowy godnych zaufania agentów AI?

## Cele nauki

Po ukończeniu tej lekcji będziesz potrafił:

- Zdefiniować wzorzec projektowy użycia narzędzi i jego cel.
- Zidentyfikować przypadki użycia, w których wzorzec ten jest stosowalny.
- Zrozumieć kluczowe elementy potrzebne do implementacji wzorca.
- Rozpoznać kwestie związane z zapewnieniem wiarygodności agentów AI korzystających z tego wzorca.

## Czym jest wzorzec projektowy użycia narzędzi?

**Wzorzec projektowy użycia narzędzi** koncentruje się na umożliwieniu LLM (Large Language Models) interakcji z zewnętrznymi narzędziami w celu realizacji konkretnych celów. Narzędzia to kod, który może być wykonywany przez agenta, aby podjąć określone działania. Narzędzie może być prostą funkcją, taką jak kalkulator, lub wywołaniem API do usługi zewnętrznej, np. sprawdzania cen akcji czy prognozy pogody. W kontekście agentów AI narzędzia są projektowane tak, aby były wywoływane przez agentów w odpowiedzi na **wywołania funkcji generowane przez model**.

## Do jakich zastosowań można go stosować?

Agenci AI mogą wykorzystywać narzędzia do realizacji złożonych zadań, pozyskiwania informacji lub podejmowania decyzji. Wzorzec użycia narzędzi jest często stosowany w scenariuszach wymagających dynamicznej interakcji z zewnętrznymi systemami, takimi jak bazy danych, usługi internetowe czy interpretery kodu. Ta zdolność jest przydatna w wielu różnych przypadkach, w tym:

- **Dynamiczne pozyskiwanie informacji:** Agenci mogą zapytywać zewnętrzne API lub bazy danych, aby pobierać aktualne dane (np. zapytania do bazy SQLite w celu analizy danych, pobieranie cen akcji lub informacji o pogodzie).
- **Wykonywanie i interpretacja kodu:** Agenci mogą uruchamiać kod lub skrypty, aby rozwiązywać problemy matematyczne, generować raporty lub przeprowadzać symulacje.
- **Automatyzacja przepływów pracy:** Automatyzacja powtarzalnych lub wieloetapowych procesów poprzez integrację narzędzi takich jak harmonogramy zadań, usługi e-mail czy potoki danych.
- **Wsparcie klienta:** Agenci mogą współpracować z systemami CRM, platformami zgłoszeń lub bazami wiedzy, aby rozwiązywać zapytania użytkowników.
- **Generowanie i edycja treści:** Agenci mogą korzystać z narzędzi takich jak korektory gramatyczne, streszczacze tekstów czy oceniacze bezpieczeństwa treści, aby wspomagać tworzenie materiałów.

## Jakie elementy/bloki konstrukcyjne są potrzebne do implementacji wzorca użycia narzędzi?

Te bloki konstrukcyjne pozwalają agentowi AI realizować szeroki zakres zadań. Przyjrzyjmy się kluczowym elementom potrzebnym do implementacji Wzorca projektowego użycia narzędzi:

- **Schematy funkcji/narzędzi:** Szczegółowe definicje dostępnych narzędzi, w tym nazwa funkcji, cel, wymagane parametry oraz oczekiwane wyniki. Schematy te umożliwiają LLM zrozumienie, jakie narzędzia są dostępne i jak tworzyć poprawne zapytania.

- **Logika wywoływania funkcji:** Określa, jak i kiedy narzędzia są wywoływane na podstawie intencji użytkownika i kontekstu rozmowy. Może to obejmować moduły planujące, mechanizmy routingu lub warunkowe przepływy decydujące o dynamicznym użyciu narzędzi.

- **System obsługi wiadomości:** Komponenty zarządzające przepływem konwersacji między wejściami użytkownika, odpowiedziami LLM, wywołaniami narzędzi i ich wynikami.

- **Framework integracji narzędzi:** Infrastruktura łącząca agenta z różnymi narzędziami, zarówno prostymi funkcjami, jak i złożonymi usługami zewnętrznymi.

- **Obsługa błędów i walidacja:** Mechanizmy radzenia sobie z niepowodzeniami wykonania narzędzi, weryfikacji parametrów oraz zarządzania nieoczekiwanymi odpowiedziami.

- **Zarządzanie stanem:** Śledzenie kontekstu rozmowy, poprzednich interakcji z narzędziami oraz danych trwałych, aby zapewnić spójność w wieloetapowych interakcjach.

Następnie przyjrzyjmy się bliżej wywoływaniu funkcji/narzędzi.

### Wywoływanie funkcji/narzędzi

Wywoływanie funkcji to podstawowy sposób, w jaki umożliwiamy dużym modelom językowym (LLM) interakcję z narzędziami. Często terminy „Funkcja” i „Narzędzie” są używane zamiennie, ponieważ „funkcje” (bloki wielokrotnego użytku kodu) są „narzędziami”, których agenci używają do realizacji zadań. Aby kod funkcji mógł zostać wywołany, LLM musi porównać żądanie użytkownika z opisem funkcji. W tym celu do LLM wysyłany jest schemat zawierający opisy wszystkich dostępnych funkcji. LLM wybiera wtedy najbardziej odpowiednią funkcję do zadania i zwraca jej nazwę oraz argumenty. Wybrana funkcja jest wywoływana, a jej odpowiedź przesyłana z powrotem do LLM, który wykorzystuje te informacje do odpowiedzi na zapytanie użytkownika.

Aby deweloperzy mogli zaimplementować wywoływanie funkcji dla agentów, potrzebne będą:

1. Model LLM obsługujący wywoływanie funkcji
2. Schemat zawierający opisy funkcji
3. Kod dla każdej opisanej funkcji

Użyjmy przykładu pobierania aktualnego czasu w mieście, aby to zilustrować:

1. **Zainicjuj LLM obsługujący wywoływanie funkcji:**

    Nie wszystkie modele obsługują wywoływanie funkcji, dlatego ważne jest, aby sprawdzić, czy używany model to potrafi. <a href="https://learn.microsoft.com/azure/ai-services/openai/how-to/function-calling" target="_blank">Azure OpenAI</a> obsługuje wywoływanie funkcji. Możemy zacząć od inicjalizacji klienta Azure OpenAI.

    ```python
    # Initialize the Azure OpenAI client
    client = AzureOpenAI(
        azure_endpoint = os.getenv("AZURE_OPENAI_ENDPOINT"), 
        api_key=os.getenv("AZURE_OPENAI_API_KEY"),  
        api_version="2024-05-01-preview"
    )
    ```

1. **Utwórz schemat funkcji:**

    Następnie zdefiniujemy schemat JSON zawierający nazwę funkcji, opis jej działania oraz nazwy i opisy parametrów funkcji. Ten schemat przekażemy do wcześniej utworzonego klienta wraz z zapytaniem użytkownika o czas w San Francisco. Ważne jest, aby zauważyć, że zwracane jest **wywołanie narzędzia**, a **nie** ostateczna odpowiedź na pytanie. Jak wspomniano wcześniej, LLM zwraca nazwę wybranej funkcji oraz argumenty, które zostaną do niej przekazane.

    ```python
    # Function description for the model to read
    tools = [
        {
            "type": "function",
            "function": {
                "name": "get_current_time",
                "description": "Get the current time in a given location",
                "parameters": {
                    "type": "object",
                    "properties": {
                        "location": {
                            "type": "string",
                            "description": "The city name, e.g. San Francisco",
                        },
                    },
                    "required": ["location"],
                },
            }
        }
    ]
    ```
   
    ```python
  
    # Initial user message
    messages = [{"role": "user", "content": "What's the current time in San Francisco"}] 
  
    # First API call: Ask the model to use the function
      response = client.chat.completions.create(
          model=deployment_name,
          messages=messages,
          tools=tools,
          tool_choice="auto",
      )
  
      # Process the model's response
      response_message = response.choices[0].message
      messages.append(response_message)
  
      print("Model's response:")  

      print(response_message)
  
    ```

    ```bash
    Model's response:
    ChatCompletionMessage(content=None, role='assistant', function_call=None, tool_calls=[ChatCompletionMessageToolCall(id='call_pOsKdUlqvdyttYB67MOj434b', function=Function(arguments='{"location":"San Francisco"}', name='get_current_time'), type='function')])
    ```
  
1. **Kod funkcji potrzebny do realizacji zadania:**

    Teraz, gdy LLM wybrał funkcję do uruchomienia, kod realizujący zadanie musi zostać zaimplementowany i wykonany. Możemy zaimplementować kod pobierający aktualny czas w Pythonie. Konieczne będzie także napisanie kodu do wyodrębnienia nazwy i argumentów z response_message, aby uzyskać ostateczny wynik.

    ```python
      def get_current_time(location):
        """Get the current time for a given location"""
        print(f"get_current_time called with location: {location}")  
        location_lower = location.lower()
        
        for key, timezone in TIMEZONE_DATA.items():
            if key in location_lower:
                print(f"Timezone found for {key}")  
                current_time = datetime.now(ZoneInfo(timezone)).strftime("%I:%M %p")
                return json.dumps({
                    "location": location,
                    "current_time": current_time
                })
      
        print(f"No timezone data found for {location_lower}")  
        return json.dumps({"location": location, "current_time": "unknown"})
    ```

    ```python
     # Handle function calls
      if response_message.tool_calls:
          for tool_call in response_message.tool_calls:
              if tool_call.function.name == "get_current_time":
     
                  function_args = json.loads(tool_call.function.arguments)
     
                  time_response = get_current_time(
                      location=function_args.get("location")
                  )
     
                  messages.append({
                      "tool_call_id": tool_call.id,
                      "role": "tool",
                      "name": "get_current_time",
                      "content": time_response,
                  })
      else:
          print("No tool calls were made by the model.")  
  
      # Second API call: Get the final response from the model
      final_response = client.chat.completions.create(
          model=deployment_name,
          messages=messages,
      )
  
      return final_response.choices[0].message.content
     ```

    ```bash
      get_current_time called with location: San Francisco
      Timezone found for san francisco
      The current time in San Francisco is 09:24 AM.
     ```

Wywoływanie funkcji jest sercem większości, jeśli nie wszystkich, wzorców użycia narzędzi przez agentów, jednak implementacja od podstaw może być czasem wyzwaniem. Jak dowiedzieliśmy się w [Lekcji 2](../../../02-explore-agentic-frameworks), frameworki agentowe dostarczają gotowe bloki konstrukcyjne do implementacji użycia narzędzi.

## Przykłady użycia narzędzi z frameworkami agentowymi

Oto kilka przykładów, jak można zaimplementować Wzorzec projektowy użycia narzędzi, korzystając z różnych frameworków agentowych:

### Semantic Kernel

<a href="https://learn.microsoft.com/azure/ai-services/agents/overview" target="_blank">Semantic Kernel</a> to otwartoźródłowy framework AI dla programistów .NET, Python i Java pracujących z dużymi modelami językowymi (LLM). Upraszcza proces wywoływania funkcji, automatycznie opisując Twoje funkcje i ich parametry dla modelu poprzez proces zwany <a href="https://learn.microsoft.com/semantic-kernel/concepts/ai-services/chat-completion/function-calling/?pivots=programming-language-python#1-serializing-the-functions" target="_blank">serializacją</a>. Obsługuje także dwukierunkową komunikację między modelem a Twoim kodem. Kolejną zaletą korzystania z frameworka agentowego takiego jak Semantic Kernel jest dostęp do gotowych narzędzi, takich jak <a href="https://github.com/microsoft/semantic-kernel/blob/main/python/samples/getting_started_with_agents/openai_assistant/step4_assistant_tool_file_search.py" target="_blank">File Search</a> oraz <a href="https://github.com/microsoft/semantic-kernel/blob/main/python/samples/getting_started_with_agents/openai_assistant/step3_assistant_tool_code_interpreter.py" target="_blank">Code Interpreter</a>.

Poniższy diagram ilustruje proces wywoływania funkcji z Semantic Kernel:

![function calling](../../../translated_images/functioncalling-diagram.a84006fc287f60140cc0a484ff399acd25f69553ea05186981ac4d5155f9c2f6.pl.png)

W Semantic Kernel funkcje/narzędzia nazywane są <a href="https://learn.microsoft.com/semantic-kernel/concepts/plugins/?pivots=programming-language-python" target="_blank">Pluginami</a>. Możemy przekształcić funkcję `get_current_time`, którą widzieliśmy wcześniej, w plugin, zamieniając ją na klasę z tą funkcją. Możemy też zaimportować dekorator `kernel_function`, który przyjmuje opis funkcji. Tworząc kernel z GetCurrentTimePlugin, kernel automatycznie serializuje funkcję i jej parametry, tworząc schemat do wysłania do LLM.

```python
from semantic_kernel.functions import kernel_function

class GetCurrentTimePlugin:
    async def __init__(self, location):
        self.location = location

    @kernel_function(
        description="Get the current time for a given location"
    )
    def get_current_time(location: str = ""):
        ...

```

```python 
from semantic_kernel import Kernel

# Create the kernel
kernel = Kernel()

# Create the plugin
get_current_time_plugin = GetCurrentTimePlugin(location)

# Add the plugin to the kernel
kernel.add_plugin(get_current_time_plugin)
```
  
### Azure AI Agent Service

<a href="https://learn.microsoft.com/azure/ai-services/agents/overview" target="_blank">Azure AI Agent Service</a> to nowszy framework agentowy zaprojektowany, aby umożliwić deweloperom bezpieczne tworzenie, wdrażanie i skalowanie wysokiej jakości, rozszerzalnych agentów AI bez konieczności zarządzania zasobami obliczeniowymi i pamięciowymi. Jest szczególnie przydatny w zastosowaniach korporacyjnych, ponieważ jest w pełni zarządzaną usługą z zabezpieczeniami klasy enterprise.

W porównaniu z bezpośrednim korzystaniem z API LLM, Azure AI Agent Service oferuje kilka zalet, w tym:

- Automatyczne wywoływanie narzędzi – nie trzeba samodzielnie parsować wywołania narzędzia, uruchamiać go i obsługiwać odpowiedzi; wszystko odbywa się po stronie serwera
- Bezpieczne zarządzanie danymi – zamiast zarządzać własnym stanem rozmowy, można polegać na wątkach przechowujących wszystkie potrzebne informacje
- Narzędzia gotowe do użycia – narzędzia do interakcji z Twoimi źródłami danych, takie jak Bing, Azure AI Search i Azure Functions.

Narzędzia dostępne w Azure AI Agent Service można podzielić na dwie kategorie:

1. Narzędzia wiedzy:
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/bing-grounding?tabs=python&pivots=overview" target="_blank">Grounding z Bing Search</a>
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/file-search?tabs=python&pivots=overview" target="_blank">File Search</a>
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/azure-ai-search?tabs=azurecli%2Cpython&pivots=overview-azure-ai-search" target="_blank">Azure AI Search</a>

2. Narzędzia akcji:
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/function-calling?tabs=python&pivots=overview" target="_blank">Function Calling</a>
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/code-interpreter?tabs=python&pivots=overview" target="_blank">Code Interpreter</a>
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/openapi-spec?tabs=python&pivots=overview" target="_blank">Narzędzia zdefiniowane przez OpenAI</a>
    - <a href="https://learn.microsoft.com/azure/ai-services/agents/how-to/tools/azure-functions?pivots=overview" target="_blank">Azure Functions</a>

Agent Service pozwala na używanie tych narzędzi razem jako `toolset`. Wykorzystuje także `threads`, które śledzą historię wiadomości z konkretnej rozmowy.

Wyobraź sobie, że jesteś agentem sprzedaży w firmie Contoso. Chcesz stworzyć agenta konwersacyjnego, który będzie odpowiadał na pytania dotyczące Twoich danych sprzedażowych.

Poniższy obraz ilustruje, jak można użyć Azure AI Agent Service do analizy danych sprzedażowych:

![Agentic Service In Action](../../../translated_images/agent-service-in-action.34fb465c9a84659edd3003f8cb62d6b366b310a09b37c44e32535021fbb5c93f.pl.jpg)

Aby użyć któregokolwiek z tych narzędzi z usługą, możemy utworzyć klienta i zdefiniować narzędzie lub zestaw narzędzi. W praktyce możemy to zaimplementować za pomocą poniższego kodu w Pythonie. LLM będzie mógł spojrzeć na zestaw narzędzi i zdecydować, czy użyć funkcji stworzonej przez użytkownika `fetch_sales_data_using_sqlite_query`, czy wbudowanego Code Interpreter, w zależności od zapytania użytkownika.

```python 
import os
from azure.ai.projects import AIProjectClient
from azure.identity import DefaultAzureCredential
from fecth_sales_data_functions import fetch_sales_data_using_sqlite_query # fetch_sales_data_using_sqlite_query function which can be found in a fetch_sales_data_functions.py file.
from azure.ai.projects.models import ToolSet, FunctionTool, CodeInterpreterTool

project_client = AIProjectClient.from_connection_string(
    credential=DefaultAzureCredential(),
    conn_str=os.environ["PROJECT_CONNECTION_STRING"],
)

# Initialize function calling agent with the fetch_sales_data_using_sqlite_query function and adding it to the toolset
fetch_data_function = FunctionTool(fetch_sales_data_using_sqlite_query)
toolset = ToolSet()
toolset.add(fetch_data_function)

# Initialize Code Interpreter tool and adding it to the toolset. 
code_interpreter = code_interpreter = CodeInterpreterTool()
toolset = ToolSet()
toolset.add(code_interpreter)

agent = project_client.agents.create_agent(
    model="gpt-4o-mini", name="my-agent", instructions="You are helpful agent", 
    toolset=toolset
)
```

## Jakie są szczególne kwestie do rozważenia przy używaniu wzorca użycia narzędzi do budowy godnych zaufania agentów AI?

Częstym problemem przy dynamicznie generowanym SQL przez LLM jest bezpieczeństwo, zwłaszcza ryzyko SQL injection lub działań złośliwych, takich jak usuwanie lub manipulowanie bazą danych. Choć te obawy są uzasadnione, można je skutecznie ograniczyć poprzez odpowiednią konfigurację uprawnień dostępu do bazy danych. W większości baz danych oznacza to skonfigurowanie bazy jako tylko do odczytu. W przypadku usług bazodanowych takich jak PostgreSQL czy Azure SQL, aplikacji powinno się przypisać rolę tylko do odczytu (SELECT).

Uruchamianie aplikacji w bezpiecznym środowisku dodatkowo zwiększa ochronę. W scenariuszach korporacyjnych dane są zazwyczaj wyodrębniane i przekształcane z systemów operacyjnych do bazy danych lub hurtowni danych tylko do odczytu, z przyjaznym schematem. Takie podejście zapewnia bezpieczeństwo danych, optymalizację pod kątem wydajności i dostępności oraz ograniczony, tylko do odczytu dostęp aplikacji.

## Dodatkowe zasoby

-
<a href="https://microsoft.github.io/build-your-first-agent-with-azure-ai-agent-service-workshop/" target="_blank">
Azure AI Agents Service Workshop</a>
- <a href="https://github.com/Azure-Samples/contoso-creative-writer/tree/main/docs/workshop" target="_blank">Warsztaty Contoso Creative Writer Multi-Agent</a>
- <a href="https://learn.microsoft.com/semantic-kernel/concepts/ai-services/chat-completion/function-calling/?pivots=programming-language-python#1-serializing-the-functions" target="_blank">Samouczek wywoływania funkcji w Semantic Kernel</a>
- <a href="https://github.com/microsoft/semantic-kernel/blob/main/python/samples/getting_started_with_agents/openai_assistant/step3_assistant_tool_code_interpreter.py" target="_blank">Interpreter kodu Semantic Kernel</a>
- <a href="https://microsoft.github.io/autogen/dev/user-guide/core-user-guide/components/tools.html" target="_blank">Narzędzia Autogen</a>

## Poprzednia lekcja

[Zrozumienie wzorców projektowych agentów](../03-agentic-design-patterns/README.md)

## Następna lekcja

[Agentic RAG](../05-agentic-rag/README.md)

**Zastrzeżenie**:  
Niniejszy dokument został przetłumaczony przy użyciu usługi tłumaczenia AI [Co-op Translator](https://github.com/Azure/co-op-translator). Mimo że dążymy do dokładności, prosimy mieć na uwadze, że automatyczne tłumaczenia mogą zawierać błędy lub nieścisłości. Oryginalny dokument w języku źródłowym powinien być uznawany za źródło autorytatywne. W przypadku informacji o kluczowym znaczeniu zalecane jest skorzystanie z profesjonalnego tłumaczenia wykonanego przez człowieka. Nie ponosimy odpowiedzialności za jakiekolwiek nieporozumienia lub błędne interpretacje wynikające z korzystania z tego tłumaczenia.