# Amazon Transcribe

## Transcribe について

Amazon Transcribe は主に以下の2種類がある。

1. S3 などに音声ファイルをおいて、それを Transcbe に投げる方式(バッチ)
2. 音声ファイルをおかずに逐次 Transcribe に投げる

### 1. S3 などに音声ファイルをおいて、それを Transcbe に投げる方式(バッチ)

このコードでは1の方を実践した。
1の方では、以下の Transcribe からは以下のレスポンスが返り、文字起こしされたテキストはレスポンスに含まれない。

<details>
<summary>レスポンス</summary>

```shell
{
  CompletionTime: undefined,
  ContentRedaction: undefined,
  CreationTime: 2022-08-14T11:20:10.742Z,
  FailureReason: undefined,
  IdentifiedLanguageScore: undefined,
  IdentifyLanguage: undefined,
  IdentifyMultipleLanguages: undefined,
  JobExecutionSettings: undefined,
  LanguageCode: 'en-US',
  LanguageCodes: undefined,
  LanguageIdSettings: undefined,
  LanguageOptions: undefined,
  Media: {
    MediaFileUri: 's3://amazon-transcribe-sample/transcribe-sample.5fc2109bb28268d10fbc677e64b7e59256783d3c.mp3',
    RedactedMediaFileUri: undefined
  },
  MediaFormat: 'mp3',
  MediaSampleRateHertz: undefined,
  ModelSettings: undefined,
  Settings: undefined,
  StartTime: 2022-08-14T11:20:10.777Z,
  Subtitles: undefined,
  Tags: undefined,
  Transcript: undefined,
  TranscriptionJobName: 'cli-sample-transcription-job4',
  TranscriptionJobStatus: 'IN_PROGRESS'
}
```

</details>

代わりにS3 に文字起こしされたテキストを含むJSON が配置される。

<details>
<summary>S3 からダウンロードできる JSON</summary>

```json
{
    "accountId": "065067241986",
    "jobName": "cli-sample-transcription-job4",
    "results": {
        "items": [
            {
                "alternatives": [
                    {
                        "confidence": "1.0",
                        "content": "Machine"
                    }
                ],
                "end_time": "0.37",
                "start_time": "0.0",
                "type": "pronunciation"
            },
            {
                "alternatives": [
                    {
                        "confidence": "1.0",
                        "content": "learning"
                    }
                ],
                "end_time": "0.71",
                "start_time": "0.37",
                "type": "pronunciation"
            },
        ],
        "transcripts": [
            {
                "transcript": "Machine learning is employed in a range of computing tasks where designing and programming explicit algorithms with good performance is difficult or infeasible. Example applications include email filtering, detection of network intruders and computer vision. Machine learning is closely related to computational statistics, which also focuses on predictions making through the use of computer. It has strong ties to mathematical optimization, which delivers methods, theory and application domains to the field."
            }
        ]
    },
    "status": "COMPLETED"
}
```
</details>


### 2. 音声ファイルをおかずに逐次 Transcribe に投げる(ストリーミング)

この場合、検証はしていないが レスポンスに文字起こしされたデータが含まれる。

## 実際のコード例

- https://github.com/tramaru/hikido/pull/20/files


# 参考資料

- [Transcribe で用意されているエンドポイントとリソース一覧](https://docs.aws.amazon.com/transcribe/latest/APIReference/Welcome.html)

