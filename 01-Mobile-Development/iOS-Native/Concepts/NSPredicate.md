Un `NSPredicate` este o clasă în iOS (parte din Foundation framework) folosită pentru a defini condiții logice cu care se filtrează colecții de date sau pentru a restricționa rezultate în căutări (de exemplu în Core Data). Practic, este o expresie care evaluează dacă un anumit obiect îndeplinește criteriile specificate și returnează un rezultat true sau false pentru fiecare obiect.

În esență, `NSPredicate` permite construirea unor filtrări complexe și dinamice pe colecții de obiecte, folosind un limbaj de tip query scris în format string, cum ar fi `"property == value"` sau `"property IN collection"`. Este folosit pentru a extrage doar elementele care îndeplinesc anumite condiții fără a itera manual prin toate elementele.

***

```
func getFilteredTransactionsPredicate() -> NSPredicate? {
        var predicates: [NSPredicate] = []
        
        // First apply view mode filtering
        switch selectedViewMode {
        case .my:
            if let userUUID = UUID(uuidString: currentUserID) {
                predicates.append(NSPredicate(format: "account.owner.id == %@", userUUID as CVarArg))
            }
        case .family:
            if let userUUID = UUID(uuidString: currentUserID) {
                predicates.append(NSPredicate(format: "account.owner.id != %@", userUUID as CVarArg))
            }
        case .all:
            // No user filtering for all view
            break
        }
        
        // Then apply account/group selection filtering
        switch selectedItem {
        case .allAccounts:
            // No additional filtering
            break
            
        case .account(let account):
            predicates.append(NSPredicate(format: "account == %@", account))
            
        case .accountGroup(let group):
            if let accounts = group.accounts as? Set<Account>, !accounts.isEmpty {
                predicates.append(NSPredicate(format: "account IN %@", accounts))
            }
            
        case .cloudKitGroup(let group):
            // Filter by accounts shared with this group
            if let accounts = group.sharedAccounts as? Set<Account>, !accounts.isEmpty {
                predicates.append(NSPredicate(format: "account IN %@", accounts))
            }
            
        case .groupMember(let member):
            predicates.append(NSPredicate(format: "account.owner == %@", member))
        }
        
        return predicates.isEmpty ? nil : NSCompoundPredicate(andPredicateWithSubpredicates: predicates)
    }
```
### Ce se întâmplă în funcția `getFilteredTransactionsPredicate()`?

Această funcție construiește o filtrare combinată (un predicat compus) pentru tranzacții, în funcție de câteva criterii:

1. **Filtrarea după modul de vizualizare (`selectedViewMode`):**
   - Dacă modul este `.my`, se filtrează doar tranzacțiile asociate conturilor al căror proprietar are `id` egal cu `currentUserID`.
   - Dacă modul este `.family`, se filtrează tranzacțiile ale conturilor al căror proprietar nu este utilizatorul curent.
   - Dacă modul este `.all`, nu se aplică filtrare după proprietar.

2. **Filtrarea după selecția contului sau grupului (`selectedItem`):**
   - Dacă este `allAccounts`, nu se adaugă filtrare suplimentară.
   - Dacă este un cont, se filtrează tranzacțiile doar pentru acel cont anume.
   - Dacă este un grup de conturi (`accountGroup`), se filtrează tranzacțiile aferente conturilor din grup.
   - Dacă este un grup CloudKit (`cloudKitGroup`), se aplică filtrarea pentru conturile partajate cu acel grup.
   - Dacă este un membru de grup (`groupMember`), se filtrează tranzacțiile ale conturilor a căror proprietar este acel membru.

La final, această listă de predicat-uri este combinată folosind `NSCompoundPredicate` cu operatorul AND, astfel încât doar tranzacțiile care îndeplinesc toate condițiile să fie filtrate.

***

### Rezumat

- `NSPredicate` este o expresie care filtrează colecții pe baza unor condiții logice.
- Funcția ta construiește un predicat compus din mai multe criterii de filtrare (mod vizualizare + selecție de cont/grup).
- Predicatul rezultat poate fi folosit pentru a restrânge tranzacțiile returnate doar la cele care corespund condițiilor definite [1][2][4][6].

Sources
[1] Predicates in Swift https://www.swiftbysundell.com/articles/predicates-in-swift
[2] Understanding Swift Predicate: A Simple Guide https://www.dhiwise.com/post/mastering-the-swift-predicate-effective-techniques
[3] NSPredicate for Swift https://www.youtube.com/watch?v=9CoUUCL03tA
[4] NSPredicate https://swiftjectivec.com/NSPredicate-Objective-C/
[5] nspredicate - How to use predicates in swift? https://stackoverflow.com/questions/43884251/how-to-use-predicates-in-swift
[6] NSPredicate | Apple Developer Documentation https://developer.apple.com/documentation/foundation/nspredicate
