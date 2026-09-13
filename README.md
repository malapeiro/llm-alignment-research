# LLM Alignment Research

Sérgio Azevedo (`malapeiro`) — Investigação independente em alinhamento e segurança de LLMs, com foco em metodologia de red-teaming e divulgação responsável.

Findings reportados a Google VRP e HackerOne. Atualmente em transição de carreira para a área de IA/segurança.

**Contacto:** sergio.azevedo.security@gmail.com

## Sobre este repositório

Este repositório reúne relatórios sanitizados de testes de alinhamento em LLMs, com foco em distinguir jailbreak efetivo, compliance textual e alucinação de conformidade. A metodologia é documentada de forma reproduzível; conteúdo potencialmente perigoso (prompts indutores, código funcional) não é publicado, seguindo boas práticas de divulgação responsável.

## Casos de estudo

- [Distinção entre Jailbreak Efetivo, Alucinação de Conformidade e Compliance Textual](./mistral-alignment-testing.md) — testes com modelos Mistral AI (texto e imagem)
- [LLM Jailbreak via Logical Compliance and Roleplay Exploitation](./logical-compliance-jailbreak.md) — bypass de guardrails através de indução lógica multi-turn

## Ética e divulgação responsável

Todos os testes são realizados em ambiente isolado, sem execução real de código nem exposição de sistemas de terceiros. Findings com processo de disclosure ainda em curso não são publicados até resolução ou reconhecimento pelo vendor.

## Licença

Este repositório está disponível sob CC BY-NC 4.0 — uso e citação livres com atribuição, sem fins comerciais (ver `LICENSE`).
