# LIFEBOAT — Greg's emergency recovery sheet (print / write this down)

**What this is:** an encrypted backup of every credential in my knowledge base (Ergeon + personal tokens,
the key that unlocks the private `kb` repo, and my own recovery items) — public ciphertext, useless
without the 8 words. Rebuilt with `~/kb/tools/lifeboat.sh --publish`. First built 2026-09-17 by ergane (erg 1963).

## THE 8 WORDS (write them here, in order — this is the only key)

    1. ________   2. ________   3. ________   4. ________
    5. ________   6. ________   7. ________   8. ________

(lower-case English words, separated by single spaces when typed; 8 EFF-Diceware words ≈ 103 bits —
a leak of the file alone is harmless, a leak of the words is total. Copies of the words live ONLY here on
paper and, sops-encrypted, in `kb/personal/secrets/lifeboat.yaml`.)

## WHERE THE FILE IS (no login needed for any of these)

1. `https://raw.githubusercontent.com/dglittle/lifeboat/main/lifeboat.age`   (public repo github.com/dglittle/lifeboat)
2. Google Drive → `kb-mirror/personal/lifeboat/lifeboat.age`   (glittle@gmail.com; mirror of the mac's ~/kb)
3. `~/kb/personal/lifeboat/lifeboat.age` on the mac and on the box (private git repo dglittle/kb)

## HOW TO OPEN IT (any Mac / Linux / Windows-WSL, ~5 minutes)

    # 1. get the file
    curl -fsSLO https://raw.githubusercontent.com/dglittle/lifeboat/main/lifeboat.age

    # 2. get 'age'  (one of: brew install age | sudo apt install age | download from github.com/FiloSottile/age/releases)

    # 3. decrypt — it asks "Enter passphrase:" → type the 8 words, single spaces, then Enter
    age -d -o lifeboat.tar.gz lifeboat.age
    tar xzf lifeboat.tar.gz && cat lifeboat/INSIDE.md      # INSIDE.md = what's in it + the recovery order

    # fallback if 'age' is unobtainable: same words, openssl instead (present on every Mac/Linux)
    curl -fsSLO https://raw.githubusercontent.com/dglittle/lifeboat/main/lifeboat.enc
    openssl enc -d -aes-256-cbc -pbkdf2 -iter 2000000 -in lifeboat.enc -out lifeboat.tar.gz   # prompts for the words

Wrong words → age says "incorrect passphrase" / openssl says "bad decrypt". Right words → `lifeboat/` folder with
`INSIDE.md`, `secrets/…`, `keys/…`. Work in a private folder; delete the plaintext when done.

## RECOVERY ORDER (details in INSIDE.md)

1. `keys/sops-age-keys.txt` → `~/.config/sops/age/keys.txt` (chmod 600) — now every `secrets/*.yaml` in the kb decrypts.
2. `secrets/personal/secrets/github.yaml` → `git clone https://dglittle:<GH_TOKEN>@github.com/dglittle/kb` — all my notes.
3. `secrets/personal/secrets/kit.yaml` → email first (Google backup codes), then password manager, then phone/2FA.
4. Rotate anything you fear leaked; rerun `tools/lifeboat.sh --publish`.

Sheet version: 2026-09-17 · the file's SHA-256 is in `SHA256SUMS` next to it (age is authenticated, so a tampered file
simply fails to decrypt).
