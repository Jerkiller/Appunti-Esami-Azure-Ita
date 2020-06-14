## Formattazione stringhe

* string.Format("{0} {1}", str, str2);
* {myString:C} => Currency in base alla cultura
* {myNumb:N} => Number con due cifre decimali e separatori
* {myNumb:N3} => Number con tre cifre decimali e separatori
* {myPerc:P2} => Percentage

* Metodi che aggiungono spazi vuoti a scopo di formattazione (PadLeft(), PadRight()) input.PadLeft(12, '-')
* Metodi che confrontano due stringhe o facilitano il confronto (Trim(), TrimStart(), TrimEnd(), GetHashcode(), proprietà Length)
* Metodi che consentono di determinare gli elementi all'interno di una stringa o anche di recuperare solo una parte della stringa (Contains(), StartsWith(), EndsWith(), Substring())
* Metodi che modificano il contenuto della stringa sostituendo, inserendo o rimuovendo parti (Replace(), Insert(), Remove())
* Metodi che trasformano una stringa in una matrice di stringhe o caratteri (Split(), ToCharArray())

## Array e matrici

* Array.Sort(a)
* Array.Reverse(a)
* Array.Clear(a, fromIndex, toIndex) => non shifta. Elimina elementi ma rimangono indici
* Array.Resize(ref arr, 3) => ridimensiona arr

Una stringa è palindroma?
```C#
bool isPalindrome(string str) {
  var arr = str.ToCharArray();
  Array.Reverse(arr);
  return str == new string(arr);
}
```
