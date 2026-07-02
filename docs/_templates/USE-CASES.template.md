# {ModuleName} — Use Cases Catalog

Trước khi thêm function/logic mới → search bảng này.

| Use Case | Port In | Mô tả nghiệp vụ | Port Out chính | Code |
|----------|---------|-----------------|----------------|------|
| `{Name}UseCase` | `I{Name}PortIn` | | | `application/use-cases/{name}.use-case.ts` |

## Ghi chú

- Presentation chỉ gọi Port In, không gọi repository trực tiếp
- Cross-module logic → port out, không import infrastructure module khác
