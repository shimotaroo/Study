## 1

```php
// 登録済みの内容をダウンロード
//CSV出力用
$out = new stdClass();
$out->senddata = "";

$fileName = 'enpuete_url_' . $nendo . '.csv';

$buffer = [];
$buffer[] = "年度";
$buffer[] = "授業管理番号";
$buffer[] = "アンケート結果公開URL";

$header = '';
if (!empty($buffer)) {
    $header = implode(",", $buffer);
}
$out->senddata .= $header . "\n";

$Executer = $this->getServiceManager()->get('DownloadEnqueteService');
$Executer->selectDBexcuter($nendo, $out);

$response = $this->getResponse();
$headers = $response->getHeaders();
$headers->addHeaderLine("Cache-Control", "public", true);
$headers->addHeaderLine("Pragma", "", true);
$headers->addHeaderLine('Content-Type', 'application/x-csv; charset=Shift_JIS');
$headers->addHeaderLine('Content-Disposition', "attachment; filename={$fileName}");
$headers->addHeaderLine('Accept-Ranges', 'bytes');
$str = \mb_convert_encoding($out->senddata, "SJIS-WIN", "UTF-8");
$headers->addHeaderLine('Content-Length', strlen($str));

$response->setContent($str);
return $response;
```

```php
<?php

namespace KdlCommon\Service\Syokuin\Csv;

use KdlCommon\Util\SyllaStringUtil;
use \stdClass;
use \Exception;
use Zend\Mvc\Exception\RuntimeException;

class DownloadEnqueteService extends AbstractDownloadCsv
{

    public function selectDBexcuter($nendo, $out)
    {
        $this->errorLogger = $this->getServiceLocator()->get('ErrorLogger');

        try {
            // ダウンロード用データ種痘k
            $rs = $this->getEntity('EnqRelation')->findBy(['jyuNendo' => $nendo]);
            // CSV出力用データ作成
            $this->setIchiran($rs, $out);
        } catch (Exception $e) {
            $this->errorLogger->err($e->getMessage());
            throw new RuntimeException($e->getMessage());
        }
        
        return true;
    }

    /**
     * 
     * @param type $req
     * @param type $rs
     * @param type $session
     * @param type $out
     * @param type $selectYear
     * @return type
     */
    protected function setIchiran($rs, $out)
    {
        foreach ($rs as $value) {
            $tmpJyuNendo = SyllaStringUtil::replaceNull($value->getJyuNendo());
            $tmpJyuKnrNo = SyllaStringUtil::replaceNull($value->getJyuKnrNo());
            $tmpEnqUrl = SyllaStringUtil::replaceNull($value->getEnqUrl());

            $buffer = [];
            $buffer[] = trim($tmpJyuNendo);
            $buffer[] = trim($tmpJyuKnrNo);
            $buffer[] = trim($tmpEnqUrl);

            $csv = implode(",", $buffer);
            $csv .= "\n";
            $out->senddata .= $csv;
        }
        return;
    }
}
```