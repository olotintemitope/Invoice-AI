# 🧾 AI Invoice Generator Agent

This project is an **AI-powered invoice assistant** that:
- Reads invoice data from a CSV or a user-supplied list.
- Generates a professional, styled invoice in PDF format.
- Dynamically applies tax, logo, due dates, and currency formatting.
- Supports multi-page rendering, with a "Balance Due" box on page 1.

Built using [LlamaIndex](https://github.com/jerryjliu/llama_index), `fpdf`, and OpenAI’s `gpt-4o-mini`.

---

## 🚀 Features

✅ Reads invoice entries from a CSV  
✅ Accepts direct list entries (e.g. `'Wed, Apr 2 : 4.4 : 50'`)  
✅ Supports custom company logo and invoice layout  
✅ Adds tax, due date, and invoice number  
✅ Multi-currency support (₦, US$, €, £, ¥)  
✅ Prevents runaway loops with tool-agent architecture  

---

## 📦 Dependencies

Install requirements with:

```bash
pip install fpdf python-dotenv llama-index nest_asyncio openai
```

---

## 🧠 Agent Tools

| Tool Name                     | Description                              |
|------------------------------|------------------------------------------|
| `read_invoice(file_path)`    | Parses a CSV and returns invoice entries |
| `generate_invoice_pdf_with_reference(...)` | Generates styled PDF from entries      |

---

## 🏗️ Usage

### 1. Setup

Create a `.env` file with your OpenAI key:

```env
OPENAI_API_KEY=sk-...
```

Your CSV file must be formatted with at least:

```csv
Date, Description, Quantity, Price
04/01/2025, Task, 4.4, 50
```

### 2. Run the Agent

Edit and run the async block:

```python
response = await agent.run("""
Use data from data/april_2025.csv and format the invoice using the layout in invoice_sample_layout.png.

Also:
- Use data/company_logo.png as the company logo.
- Apply a tax of 0% to the total cost.
- Due date is 5 days from today.
- Generate the invoice number starting with INV_ followed by a random 6-digit number.
- Use the following entries instead of the CSV:
[
  'Item 1 : 4.4 : 50',
  'Item 2 : 5.0 : 50',
  ...
]
- Generate the invoice in `ngn` currency.
""")
```

### 3. Output

You will get a response like:

```json
{
  "pdf_path": "styled_invoice.pdf",
  "total_cost": 3950.0,
  "invoice_number": "INV_348210",
  "status": "Invoice generated successfully."
}
```

---

## 🌍 Supported Currencies

| Code | Symbol |
|------|--------|
| usd  | US$    |
| eur  | €      |
| gbp  | £      |
| ngn  | ₦      |
| jpy  | ¥      |

To use, simply pass `currency="eur"` in the prompt.

---

## 🧠 Architecture

Powered by:

- 🧠 `FunctionAgent` from LlamaIndex
- 🛠️  `FunctionTool` wrappers
- 📄 `fpdf` for PDF rendering
- 🔁 Controlled with `max_iterations=2` to avoid infinite tool loops

---

## 📂 Folder Structure

```
.
├── data/
│   ├── april_2025.csv
│   ├── company_logo.png
│   └── invoice_sample_layout.png
├── invoice_debug.log
├── styled_invoice.pdf
└── main.py
```

---

## 📄 License

MIT — use it commercially or for learning!  
Credit appreciated: [Temitope Olotin](https://www.linkedin.com/in/temitopeolotin/)