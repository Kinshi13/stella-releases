# Catálogo do Stella Hub

`stable.json` é o catálogo assinado que o **Stella Hub 0.2.0+** consulta quando o usuário toca em **Verificar atualizações**.
O Hub só faz essa consulta quando o usuário pede: não há verificação automática, agendamento ou notificações.

## Formato

```json
{"format":"stella-catalog-envelope-v1","keyId":"stella-catalog-1","payloadBase64":"…","signatureBase64":"…"}
```

- `payloadBase64`: os bytes exatos do catálogo (JSON, UTF-8), com `schemaVersion`, `catalogRevision`, `generatedAt`, `expiresAt` e `apps`.
- `signatureBase64`: assinatura **ECDSA P-256 / SHA-256** (DER) desses bytes.
- `keyId`: seleciona uma chave pública **já embutida no Hub**. O catálogo não consegue trazer uma chave própria.

Chave pública `stella-catalog-1` (X.509 SubjectPublicKeyInfo, base64):

```
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEZtlCQF35IDCn9sMG934Bmi6CrJNxIRtvJulHFB6fvVxkYrfi/T+KxDP5rykpkCe5fncSiVkf0SFThqk2TuMJPQ==
```

SHA-256 da chave pública: `61d9d7334c48a590affb97b19dc72f65103b434395828c0bd165afbd334c802d`

## Regras que o Hub aplica

- A assinatura, o formato, a estrutura (sem campos duplicados) e os limites de tamanho precisam bater. Senão, o catálogo é recusado e nada muda no aparelho.
- A revisão só sobe. Uma revisão igual à já aceita só vale com os mesmos bytes.
- A validade é de no máximo 30 dias. Um catálogo vencido mantém os apps visíveis e abríveis, mas bloqueia novas instalações até ser renovado.
- O catálogo só descreve apps que o Hub já conhece, com os certificados de assinatura que o Hub já confia. App novo ou chave nova exige uma nova versão do Hub.
- Os APKs ficam em releases de tag fixa deste repositório. Um código de versão nunca muda de bytes, e a versão nunca regride.

O histórico de revisões é o histórico Git deste arquivo.
