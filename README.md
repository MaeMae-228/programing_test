# programing_test

log.csvはテストデータである

コード説明
・設問1
◦dataread関数(4行目～)
ログファイルを読み込むための関数

◦main部分(9行目～)
trouble_start_dtesは、故障が発生した際、そのサーバーアドレスをキー、故障が発生した時間を値として保存する

13～19行目においては、ログを1行ずつ読み込み、コンマの位置により日時・サーバーアドレス・応答結果に分けている

そして21行目からは、pingがタイムアウトしているか否かにより条件分岐を行っている
(1)タイムアウトしている場合
そのサーバーアドレスがtrouble_start_dtesのキーに存在するか否か(すなわちその前の時点において故障していたか否か)を確認する
存在しない場合にはその時点において故障したと判断し、trouble_start_dtesにサーバーアドレスをキー、時刻を値として格納する
(2)応答がある場合
そのサーバーアドレスがtrouble_start_dtesのキーに存在するか否か(すなわちその前の時点において故障していたか否か)を確認する
存在する場合には、その時点において故障状態から復旧したと判断する　
trouble_start_dtesのサーバーアドレスに対応する形で格納されている、故障が発生した時間を取得し故障期間を求める
そして、復旧したことと故障期間を知らせる分を出力する

最後に31行目からは、trouble_start_dtesに値が存在するか否か(すなわちその時点において故障しているサーバーがあるか否か)により条件分岐を行っている
(1)値が存在する場合
trouble_start_dtesからキーのみを取り出し、その時点において故障しているサーバーアドレスを出力する
(2)値が存在しない場合
故障がない旨を伝える分を出力する


・設問2
設問1との差分部分を中心に記載する

11行目から
trouble_countは、pingのタイムアウトの連続回数を、対応するサーバーアドレスをキーとして格納する

24行目からは、pingがタイムアウトしているか否かにより条件分岐を行っている
(1)タイムアウトしている場合
そのサーバーアドレスがtrouble_countのキーに存在するか否か(すなわちその前の時点において故障していたか否か)を確認する
　(1-1)存在する場合には、以前から故障していたと判断し、trouble_countのサーバーアドレスをキーとして、対応する値であるpingのタイムアウトの連続回数の値を1だけ加算する
 　さらに、その連続回数の値によって条件分岐を行う
  　　　(1-1-1)連続回数がNと等しい場合には、その時点において故障したと判断し、trouble_start_dtesにサーバーアドレスをキー、時刻を値として格納する
     　 (1-1-2)連続回数がNと異なる場合には、以前から故障していた、またはタイムアウトの連続回数が故障と判断される基準に達していないと判断し、特段の処理は行わない
　(1-2)存在しない場合にはその時点において故障したと判断し、trouble_countにサーバーアドレスをキー、そして値として1(pingのタイムアウトの連続回数)を格納する
(2)応答がある場合
そのサーバーアドレスがtrouble_start_dtesのキーに存在するか否か(すなわちその前の時点において故障していたか否か)を確認する
　(2-1)存在する場合には、その時点において故障状態から復旧したと判断する　
 　trouble_start_dtesのサーバーアドレスに対応する形で格納されている、故障が発生した時間を取得し故障期間を求める
   そして、復旧したことと故障期間を知らせる分を出力する
  (2-2)存在しない場合には、前の時点においても故障状態ではなかったと判断する
  　trouble_countにそのサーバーアドレスが存在する(故障には至らなかったが、前の時点までタイムアウトが生じていた)場合、trouble_countからそのサーバーアドレスと対応する値を消去する
   
   
・設問3
設問2との差分部分を中心に記載する

response_timesは、サーバーアドレスをキーとして、対応する過去m回の応答時間を格納したリストを値として格納する
overload_start_dtesは、過負荷状態であるサーバーアドレスをキーとして、過負荷状態が始まった時間を値として格納する

37,8行目は、タイムアウトした場合、応答時間の平均を計算することが不可能となるため、直近の応答時間を格納するリストを削除している

49～70行目
まずresponse_timesのキーにサーバーアドレスが存在しない場合には、そのサーバーアドレスをキーとするリストを作成する
そのリストの最後の要素として、応答時間を加える
そして、リストの要素数がmを超えたになった場合、最も古い記録の応答時間(リストの最初の要素)を削除する
リストの要素数がmであれば、リストの平均値(直近m回の平均応答時間)を導出する

59行目からは、直近m回の平均応答時間とtの大小関係により条件分岐を行う
(1)直近m回の平均応答時間がtより大きい場合
overload_start_dtesにサーバーアドレスが存在するか否か(前の時点においても過負荷状態であったか否か)によって条件分岐を行う
　(1-1)overload_start_dtesにサーバーアドレスが存在する場合
 　その時点において過負荷状態になったと判断し、過負荷状態になった旨の文を出力し、その時点の時刻をoverload_start_dtesに格納する
  (1-2)overload_start_dtesにサーバーアドレスが存在しない場合
  以前から過負荷状態であると判断し、その時点における過負荷状態の継続期間を算出・出力する
(2)直近m回の平均応答時間がt以下の場合
overload_start_dtesにサーバーアドレスが存在するか否か(前の時点においても過負荷状態であったか否か)によって条件分岐を行う
　(1-1)overload_start_dtesにサーバーアドレスが存在する場合
 　その時点において過負荷状態が緩和された判断し、過負荷状態が緩和された旨の文と、過負荷状態であった期間を出力する　また、overload_start_dtesから対応するサーバーアドレスを削除する
  (1-2)overload_start_dtesにサーバーアドレスが存在しない場合
  以前から過負荷状態でなかったと判断し、特段の処理は行わない


・設問4
同じサブネット内であることを判別するプログラムを組み立てることができず、コードがかけていない
