## `Service`
Pliki na dysku w kontenerze są efemeryczne, co stwarza pewne problemy w przypadku nietrywialnych aplikacji działających w kontenerach. Jednym z problemów jest utrata plików w przypadku awarii kontenera. Kubelet ponownie uruchamia kontener, ale z czystym stanem. Drugi problem występuje podczas udostępniania plików między kontenerami działającymi razem w pode. Abstrakcja woluminu Kubernetes rozwiązuje oba te problemy.

Przykład volumenu używanego w AWS EBS`Service` Kubernetes to logiczna abstrakcja dla wdrożonej grupy `POD` w klastrze (które pełnią tę samą funkcję).

Ponieważ pody są efemeryczne, usługa umożliwia grupie podów, które zapewniają określone funkcje (usługi sieciowe, przetwarzanie obrazów itp.) przypisanie nazwy i unikalnego adresu IP (clusterIP). Dopóki usługa działa pod tym adresem IP, nie zmieni się. Usługi określają również zasady ich dostępu.