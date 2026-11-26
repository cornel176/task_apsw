# TaskFlow — jurnal de modificări și testare

## Mediu
- Sistem de operare: Windows
- Node.js: v24.20.0
- Browser: browserul integrat cu Playwright în VS Code
- Comenzi: `npm run api`, `npm run dev`, `npm run build`
- `package.json` nu definește un script de teste automatizate.

## Probleme identificate și remediate

| Nr. | Pași de reproducere | Comportament observat | Comportament așteptat | Soluție aplicată | Verificare |
|---|---|---|---|---|---|
| 1 | Deschide dashboard-ul cu datele inițiale. | Numărul „In progress” nu corespundea statusului din API din cauza diferenței de majuscule. | Să numere sarcinile cu statusul `In progress`. | Alinierea comparației la valoarea folosită de API. | Verificat în browser: 2 sarcini „In progress”. |
| 2 | Din dashboard apasă „View all”; din detaliile unei sarcini, șterge sarcina. | Unele acțiuni foloseau ruta inexistentă `/task`. | Navigare către `/tasks`. | Corectarea ambelor rute către `/tasks`. | Ștergerea din interfață a revenit la lista `/tasks`; linkul din dashboard folosește ruta corectă. |
| 3 | Creează o sarcină cu titlul format numai din spații. | Titlul putea trece de validarea care verifica doar șirul gol. | Formularul să respingă titlul fără caractere vizibile. | Validarea cu `trim()` și salvarea titlului curățat. | Verificat în browser: apare „A task title is required.” |
| 4 | Editează o sarcină și schimbă proiectul asociat. | ID-ul proiectului putea fi salvat ca text, deși listele îl compară ca număr. | Sarcina să rămână asociată proiectului selectat. | Conversia lui `projectId` la număr înainte de actualizare. | Verificat prin editare în browser și răspuns API cu `projectId: 2`. |
| 5 | În lista de sarcini selectează statusul „In progress”. | Filtrul compara cu `In Progress` și omitea sarcinile existente. | Să afișeze sarcinile cu statusul selectat. | Compararea directă cu valoarea selectată. | Filtrele combinate „High” + „In progress” au afișat 1 rezultat. |
| 6 | Deschide „Projects” și apasă „View project”. | Linkul folosea `/project/:id`, rută care nu există. | Deschiderea paginii `/projects/:id`. | Corectarea rutei proiectului. | Verificat în browser: s-a deschis `/projects/1` și titlul „Website redesign”. |
| 7 | Caută un responsabil cu litere mici sau cu altă combinație de majuscule. | Căutarea titlurilor ignora majusculele, dar cea a responsabililor nu. | Căutarea să fie insensibilă la majuscule pentru ambele câmpuri. | Normalizarea responsabilului și interogării la litere mici. | Căutarea `aNa`, combinată cu filtrele, a găsit sarcina Anei Popescu. |
| 8 | Încarcă pagina Projects când API-ul nu răspunde. | Promisiunea eșuată nu era tratată, iar pagina putea părea goală. | Afișarea unei erori clare. | Tratarea erorii și afișarea unui mesaj accesibil. | Build verificat; simularea manuală a indisponibilității API-ului nu a fost executată. |
| 9 | Deschide dashboard-ul și bara de navigare într-o altă zi. | Interfața afișa data fixă 15 octombrie 2026. | Afișarea datei curente. | Generarea etichetelor de dată la randare. | Verificat în browser: ambele afișau 26 noiembrie 2026. |
| 10 | Încarcă dashboard-ul când API-ul nu răspunde. | Eroarea nu era tratată și statisticile puteau părea zero. | Afișarea erorii, fără statistici care sugerează un rezultat valid. | Tratarea erorii și afișarea unui mesaj în locul dashboard-ului. | Build verificat; simularea manuală a indisponibilității API-ului nu a fost executată. |

## Funcționalitate nouă: filtrare după prioritate
- Am adăugat un control accesibil „Filter by priority” cu „All priorities”, Low, Medium și High.
- Prioritatea funcționează împreună cu căutarea și statusul; „Clear filters” resetează toate criteriile.
- Verificare în browser: High a afișat 3 din 7 sarcini; High + In progress a afișat 1; o căutare fără rezultate a afișat starea goală; resetarea a readus toate cele 7 sarcini.

## Verificări finale
- [x] `npm run build` — build-ul de producție a reușit.
- [x] CRUD prin interfață — creare, editare și ștergere verificate cu sarcini temporare; datele inițiale au rămas la 7 sarcini.
- [x] Navigare — pagina de detalii a proiectului și revenirea după ștergerea unei sarcini verificate.
- [x] Căutare și filtre combinate verificate.
- [x] Scenariu fără rezultate și resetarea filtrelor verificate.
- [x] Formular cu titlu invalid verificat.
- [ ] Simularea erorilor API în browser.
- [ ] Pull Request creat; jurnalul poate fi inclus în descrierea PR-ului.
