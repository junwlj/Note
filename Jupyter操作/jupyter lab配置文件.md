# Jupyter Lab常用配置说明

修改**Jupyter Lab/Jupyter NoteBook**配置文件，美化*Jupyter*编辑工具。

```json
{
    // Notebook
    // @jupyterlab/notebook-extension:tracker
    // Notebook settings.
    // **************************************

    // Code Cell Configuration
    // The configuration for all code cells.
    // 设置每一个cell样式
    "codeCellConfig": {
        "autoClosingBrackets": true, 
        "fontFamily": null,
        "fontSize": null,
        "lineHeight": null,
        "lineNumbers": false,  // 显示行号
        "lineWrap": "off",    // 设置代码自动换行
        "matchBrackets": true,
        "readOnly": false,
        "insertSpaces": true,
        "tabSize": 4,
        "wordWrapColumn": 80,
        "rulers": [],
        "codeFolding": false,
        "lineWiseCopyCut": true
    },

    // Default cell type
    // The default type (markdown, code, or raw) for new cells
    "defaultCell": "code",

    // Shut down kernel
    // Whether to shut down or not the kernel when closing a notebook.
    "kernelShutdown": false,

    // Markdown Cell Configuration
    // The configuration for all markdown cells.
    "markdownCellConfig": {
        "autoClosingBrackets": false,
        "fontFamily": null,
        "fontSize": null,
        "lineHeight": null,
        "lineNumbers": false,
        "lineWrap": "on",
        "matchBrackets": false,
        "readOnly": false,
        "insertSpaces": true,
        "tabSize": 4,
        "wordWrapColumn": 80,
        "rulers": [],
        "codeFolding": false,
        "lineWiseCopyCut": true
    },

    // Raw Cell Configuration
    // The configuration for all raw cells.
    "rawCellConfig": {
        "autoClosingBrackets": false,
        "fontFamily": null,
        "fontSize": null,
        "lineHeight": null,
        "lineNumbers": false,
        "lineWrap": "on",
        "matchBrackets": false,
        "readOnly": false,
        "insertSpaces": true,
        "tabSize": 4,
        "wordWrapColumn": 80,
        "rulers": [],
        "codeFolding": false,
        "lineWiseCopyCut": true
    },

    // Recording timing
    // Should timing data be recorded in cell metadata
    "recordTiming": false,

    // Scroll past last cell
    // Whether to be able to scroll so the last cell is at the top of the panel
    "scrollPastEnd": true
}
```

