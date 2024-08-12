1. 首先将矢量图保存为 PDF 格式。
2. 使用 `pdfcrop` 工具裁剪 PDF 页面空白：
   ```sh
   pdfcrop <input.pdf> [output.pdf]
   ```
3. 在 `.tex` 文件中使用 graphicx 包像导入普通图片一样导入 PDF 图片