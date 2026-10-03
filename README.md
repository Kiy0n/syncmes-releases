# syncmes-releases

Feed de versões do SyncMES: um único arquivo, [`versions.json`](versions.json), com as versões publicadas e o que
cada instalação precisa para atualizar.

## Quem escreve e quem lê

- **Escreve:** só o workflow de release do repositório principal. A cada tag `vX.Y.Z` ou `vX.Y.Z-rc.N`, ele
  acrescenta uma entrada e recalcula os canais. Os commits aparecem como `release: <versão>`.
- **Lê:** o `update.sh` de cada instalação, sem autenticação, por
  `https://raw.githubusercontent.com/Kiy0n/syncmes-releases/main/versions.json`. Ele usa o feed para decidir se a
  atualização é permitida e qual digest baixar. As imagens ficam no GHCR, que exige credencial.

## Formato

```jsonc
{
  "schema": 1,
  "product": "syncmes",
  "channels": { "stable": "v0.7.1", "prerelease": "v0.7.1" },  // maior estável / maior de todas
  "generated_at": "…",
  "versions": [{                                               // da mais nova para a mais antiga
    "version": "v0.7.1",
    "released_at": "…",
    "prerelease": false,
    "yanked": false, "yanked_reason": null,                    // versão retirada não é instalada
    "min_upgrade_from": null,                                  // versão mínima de origem, se houver
    "migrations": { "count": 0, "data_dependent": false, "preflight_sql": [] },
    "images": { "backend": { "ref": "…", "digest": "sha256:…" }, "frontend": {…}, "pdf-service": {…}, "bundle": {…} },
    "notes_url": "https://github.com/Kiy0n/syncmes/releases/tag/v0.7.1"
  }]
}
```

## Não edite à mão

O arquivo é gerado. Versões publicadas são imutáveis, e o gerador recusa repetir uma versão. Uma edição manual
muda o que as instalações aceitam e baixam na próxima atualização. Por isso, qualquer mudança fora do workflow,
inclusive retirar uma versão (`yanked`), passa antes pelo mantenedor.
