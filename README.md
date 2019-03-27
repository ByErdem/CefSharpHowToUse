CefSharp Nasıl Kullanılır
Burada cefsharp sürümünün kullanıldığını ve parametrelerin sürüme bağlı olarak değiştiğini belirtmek gerekir.
cefsharp: 57.0.0.0
CefSharp.IJsDialogHandler arabirimini uygulamak için JsDialogHandler sınıfını ekleyin

class JsDialogHandler : CefSharp.IJsDialogHandler
    {

        public void OnDialogClosed(CefSharp.IWebBrowser browserControl, CefSharp.IBrowser browser)
        {

        }

        public bool OnJSBeforeUnload(CefSharp.IWebBrowser browserControl, CefSharp.IBrowser browser, string message, bool isReload, CefSharp.IJsDialogCallback callback)
        {
            return true;
        }

        public bool OnJSDialog(CefSharp.IWebBrowser browserControl, CefSharp.IBrowser browser,string originUrl, CefSharp.CefJsDialogType dialogType, string messageText, string defaultPromptText,CefSharp.IJsDialogCallback callback, ref bool suppressMessage)
        {
            switch (dialogType)
            {
                case CefSharp.CefJsDialogType.Alert:
                    MessageBox.Show(messageText, "Alert");
                    suppressMessage = true;
                    return false;
                case CefSharp.CefJsDialogType.Confirm:
                    var dr = MessageBox.Show(messageText, "Confirm", MessageBoxButtons.YesNo);
                    if (dr == DialogResult.Yes)
                    {
                        callback.Continue(true, string.Empty);
                        suppressMessage = false;
                        return true;
                    }
                    else
                    {
                        callback.Continue(false, string.Empty);
                        suppressMessage = false;
                        return true;
                    }
                    break;
                case CefSharp.CefJsDialogType.Prompt:
                    MessageBox.Show("Sistem, bilgi istemi kutusunu bilgi istemi biçiminde desteklemiyor", "Prompt");
                    break;
                default:
                    break;
            }
            return false;
        }

        public void OnResetDialogState(CefSharp.IWebBrowser browserControl, CefSharp.IBrowser browser)
        {

        }
    }
    
        private void Form1_Load(object sender, EventArgs e)
        {
            var browser = new CefSharp.WinForms.ChromiumWebBrowser("http://192.168.41.120/QDMSV5_520_AUTO/QDMSNET/BSAT/Logon.aspx")
            {
                BrowserSettings =
                            {
                                DefaultEncoding = "UTF-8"
                            },
                JsDialogHandler = new JsDialogHandler(),
                Dock = DockStyle.Fill,
            };
            this.Controls.Add(browser);
        }
