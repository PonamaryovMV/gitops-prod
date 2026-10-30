# gitops-prod — ФП qa/at (prod)

PROD-контур ФП `qa/at` в том же формате, что и [gitops-main](https://github.com/PonamaryovMV/gitops-main):

```
qa/at/<сервис>/prod/
├── Chart.yaml
├── qa-at-<сервис>-prod.yaml   values PROD
└── release.yaml               версия — пишет Kargo (стейдж prod)
qa/at/release-exclude.yaml     срочные исключения сервисов из PROD — пишет Jenkins ExcludeService
```

- `release.yaml` обновляет только Kargo при Promote в `prod` — из Freight, прошедшего DAST на `uat`.
- `release-exclude.yaml`: сервисы из списка Kargo не берёт из Freight, а возвращает их `release.yaml` на коммит `restoreFrom`
  (состояние PROD до релиза). Записи добавляет `ExcludeService MODE=urgent`, снимает `MODE=clear`.
- Application'ы Argo CD: `qa-at-<сервис>-prod` в неймспейсе `qa-at-prod` (ApplicationSet `workloads-prod` из [infra](https://github.com/PonamaryovMV/infra)).
