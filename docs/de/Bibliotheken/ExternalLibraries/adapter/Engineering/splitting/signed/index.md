# Vorzeichenbehaftete Aufteilungsbausteine (signed splitting)

Diese Bibliothek enthält Bausteine zum Aufteilen vorzeichenbehafteter Signale (`SINT`, `INT`, `DINT`, `LINT`, `REAL`, `LREAL`) in zwei positive Betragskomponenten (`NEG_MAG` und `POS_MAG`).

## Basic FBs (Reine Berechnung)

- [SPLIT_SIGNED_SINT](./SPLIT_SIGNED_SINT.md) – Aufteilung für `SINT`
- [SPLIT_SIGNED_INT](./SPLIT_SIGNED_INT.md) – Aufteilung für `INT`
- [SPLIT_SIGNED_DINT](./SPLIT_SIGNED_DINT.md) – Aufteilung für `DINT`
- [SPLIT_SIGNED_LINT](./SPLIT_SIGNED_LINT.md) – Aufteilung für `LINT`
- [SPLIT_SIGNED_REAL](./SPLIT_SIGNED_REAL.md) – Aufteilung für `REAL`
- [SPLIT_SIGNED_LREAL](./SPLIT_SIGNED_LREAL.md) – Aufteilung für `LREAL`

## Adapter-Wrapper Composite FBs (Mit D-Flip-Flop Event-Filterung)

- [AS_SPLIT_SIGNED](./AS_SPLIT_SIGNED.md) – Adapter-Wrapper für `SINT` (`AS`)
- [AI_SPLIT_SIGNED](./AI_SPLIT_SIGNED.md) – Adapter-Wrapper für `INT` (`AI`)
- [ADI_SPLIT_SIGNED](./ADI_SPLIT_SIGNED.md) – Adapter-Wrapper für `DINT` (`ADI`)
- [ALI_SPLIT_SIGNED](./ALI_SPLIT_SIGNED.md) – Adapter-Wrapper für `LINT` (`ALI`)
- [AR_SPLIT_SIGNED](./AR_SPLIT_SIGNED.md) – Adapter-Wrapper für `REAL` (`AR`)
- [ALR_SPLIT_SIGNED](./ALR_SPLIT_SIGNED.md) – Adapter-Wrapper für `LREAL` (`ALR`)
