Molti aspetti del comportamento di TeXmacs possono essere cambiati, anche con l'aggiunta di nuove funzionalità, aggiungendo codice Scheme ai file di inizializzazione. Questo wiki post raccoglie alcuni "snippet" che in questo senso adattano il comportamento di TeXmacs secondo le vostre preferenze.

Vi invitiamo ad aggiungere i vostri snippet. Non devono essere perfetti. Ci piacerebbe che gli utenti migliorassero ed estendessero il codice contribuito dagli altri. Per progammi più lunghi il posto più giusto è la "TeXmacs Forge".

# Come editare i file di inizializzazione di TeXmacs

Gli snippets possono essere copi-incollati in uno dei vostri file di inizializzazione di TeXmacs. I titoli di ogni sezione indicano il file nel quale vanno messi.

Per editare i file nell editor di TeXmacs, come primo passo abilitate<br>`Tools→Developer Tool`; una volta fatto questo:

- Per il codice per **`my-init-texmacs.scm`**:

    Cliccate su `Developer → Open my-init-texmacs.scm`.

- Per il codice per **`my-init-buffer.scm`**:

    Cliccate su `Developer → Open my-init-buffer.scm`.

Incollate lo snippet. Per incollare codice da questa pagina all'editor di TeXmacs può essere necessario usare `Edit → Paste from → Html` in TeXmacs (anche `Edit → Paste from → Verbatim` funziona).

Salvato il file editato.

Infine, perché la customizzazione venga riconosciuta da TeXmacs:

- Se avete cambiato **`my-init-texmacs.scm`**:

    Chiudete e riaprite TeXmacs.

- Se avete cambiato **`my-init-buffer.scm`**:

    Aprite un file (tenete conto che certi snippet agiscono solo su file **nuovi** non su file già esistenti)
