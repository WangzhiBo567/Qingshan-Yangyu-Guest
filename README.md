# Qingshan-Yangyu-Guest
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>PDF转换工具</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.7.570/pdf.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
</head>
<body>
    <h1>PDF转换工具</h1>
    <input type="file" id="pdf-file" accept=".pdf">
    <button id="convert-btn">转换为图片</button>
    <div id="pdf-container"></div>

    <script>
        // 获取PDF文件并显示
        $('#pdf-file').on('change', function() {
            var file = this.files[0];
            var reader = new FileReader();
            reader.onload = function(e) {
                var buffer = e.target.result;
                PDFJS.getDocument(buffer).promise.then(function(pdf) {
                    for (var i = 1; i <= pdf.numPages; i++) {
                        pdf.getPage(i).then(function(page) {
                            var canvas = document.createElement('canvas');
                            var context = canvas.getContext('2d');
                            var viewport = page.getViewport({ scale: 1 });
                            canvas.width = viewport.width;
                            canvas.height = viewport.height;
                            page.render({ canvasContext: context, viewport: viewport }).promise.then(function() {
                                $('#pdf-container').append(canvas);
                            });
                        });
                    }
                });
            };
            reader.readAsArrayBuffer(file);
        });

        // 转换为图片
        $('#convert-btn').on('click', function() {
            var canvasList = $('#pdf-container canvas');
            for (var i = 0; i < canvasList.length; i++) {
                var canvas = canvasList[i];
                var image = new Image();
                image.src = canvas.toDataURL('image/png');
                $('#pdf-container').append(image);
            }
        });
    </script>
</body>
</html>
