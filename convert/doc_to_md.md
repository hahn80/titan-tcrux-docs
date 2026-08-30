# Convert Doc to MD

jobs:

```json
[
  {
    "task": "converting",
    "action": "doc_to_markdown",
    "kwargs": {
      "input_file": "/data/TestData/docs/filename.ext",
      "output_file": "/tmp/tmpq1kj7lgb/output.md"
    }
  }
]
```

Supported the following formats (file types)
- doc epub odp odt ppt rtf xlsb
- csv docx ods pdf pptx xls xlsx

Hashmap return for java to keep track:
{'status': 'success'}

or

{'error': 'page 6 of 6 needs OCR'}



