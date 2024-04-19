# Dokumentacja aplikacji „Ochroniarz” 

## Opis uruchomienia Aplikacji
Ochroniarz został stworzony w notebook’u ipynb w języku python. Do uruchomiania aplikacji wymagane jest środowisko python(osobiście korzystałem z wersji pythona 3.12)  oraz IDE onsługujące pliki ipynb np. Visual Studio Code(z którego korzystałem). Do poprawnego działania aplikacji wymagane są następujące biblioteki:
accelerate==0.29.2
langchain==0.1.16
langchain_community==0.0.33
langchain_core==0.1.44
langchain_experimental==0.0.57
presidio_analyzer==2.2.354
pysentimiento==0.7.3
textblob==0.18.0.post0

Spis potrzebnych bibliotek znajduję się również w pliku „requirements.txt”.

## Opis działania aplikacji
Aplikacja po wprowadzeniu do niej treści dokumentu za pomocą biblioteki Microsoft Presidio zanonimizuje dane poufne oraz zastępuje je Placeholderami, następnie wyświetla dokument po anonimizacji oraz słownik, w którym znajdują się zanimizowane dane. Anonimizacja jest odwracalna. Następnie za pomocą localnego LLM(Large Language Model) Ollam’y sprawdza, czy w dokumencie są przekleństwa, mowa nienawiści i dane o stanie zdrowia, po czym je zwraca. Kolejnym krokiem jej działania jest zastąpienie tych danych Placeholderami i ponowne wyświetlenie całkowicie zanonimizowanego dokumentu. Ostatnim krokiem działania aplikacji jest wyznaczenie współczynnika mowy nienawiści oraz na podstawie sentymentu sprawdzenie, czy tekst jest mową nienawiści.

## Opis kodu aplikacji
 ![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/5fb700e1-f830-4b7e-8cca-1d10a940616d)

Zmienna anonymizer wykorzystuje PresidioReversibleAnonymizer do anonimizacji danych. Faker jest ustawiony na false, ponieważ służy on do zastąpienia danych innymi fałszywym, a aplikacja ma te dane zastąpić placeholderami. 
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/cf355861-bbf6-4921-b1a4-b3e648d4ff8b)
 
Podstawowy anonymizer nie obsługuje numeru polskiego dowodu oraz numeru konta bankowego oddzielonego spacjami. Powyższy kod dodaje te szablony.
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/6c4a4a93-cf86-463a-8818-009e950b0b55)
 
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/caadf354-e023-41aa-909c-dd9cb741996c)

 
Funkcja print_colored_pii służy do wyświetlenia placeholderów na inny kolor w celu lepszej czytelności.
anonymizer.anonymize(document_content) anomizuje dane dokumentu.
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/7d4490f7-1fcf-4c3a-bf4c-97762ea38044)
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/9c012f49-5285-4503-abf6-cba3fd631fc3)

anonymizer.deanonymizer_mapping przechowuje słownik z zanonimizowanymi danymi. Jeśli słownik jest pusty oznacza to, że nie zostały wykryte żadne dane poufne.

![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/d7e3a410-5aa5-4efb-beed-cdf2d5a37a0e)

Anonimizacja jest odwracalna.
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/55abc37d-6431-4404-bc8b-88637ebc78c7)

 
Ollama jest wykorzystywana do wyszukania i znalezienia przekleństw, mowy nienawiści i informacji o stanie zdrowia. Zwraca string z nazwami w cudzysłowach.  
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/dc2473b0-2fa4-4e35-a1e7-91f687ca2171)

 ![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/b5d6192d-c7e2-478e-be23-a0b6768af25c)


Następnie te dane są wyciągane z stringa.
 
 ![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/81c148f1-3cb0-4c28-b15c-219165603a69)

Funkcja replace_hate_speach zamienia wykryte dane na Placeholdery.
 ![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/bbc80f9f-8b06-4213-b824-1a3f191fcbd3)
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/34c01b2b-d80d-4c27-bb46-a0ddc164b56a)

 
Za pomocą analyzer’a wyznaczane są współczynniki mowy nienawiści. Biblioteka pysentimiento nie wspiera języka polskiego, więc innym rozwiązaniem było wyznaczenie mowy nienawiści na podstawie sentymentu.
![image](https://github.com/Mydlyk/Ochroniarz/assets/65900710/2baeabbb-eb5b-4d99-a3e9-9bd99fef4b3c)
 

