# sphangon-x68k

X68000版 SUPER HANGON に関する覚書

---

## はじめに

この覚書は自分のお気に入りのX68000用ゲームソフトの一つである、SUPER HANGON (1987 SEGA / 1988 移植SPS) に関するメモです。

以下について記述しています。

* テストモード
* MIDI
* BGMデモモード
* デカキャラパッチ
* ハードディスクインストール 
* Phantom X

<img src='images/sph2.jpeg'/>

---

## テストモード

タイトル画面またはデモ画面表示中に SHIFT + HELP キーを押すとテストモードに入ることができます。

<img src='images/sph1.jpeg'/>

サウンドモードでBGM/SEを選択して聴くことができたり、ゲーム難易度の調整もできます。
デフォルトの難易度は一番易しいモードになっているようです。ただ、このメニューでの調整により実際に難易度が変わるのかは詳しく確認していません。

---

## MIDI

このソフトはMIDIボードを拡張スロットに装着していると、外部MIDI音源(MT-32,CM-64などのRoland LA音源)を鳴らすことができます。

ただし、内蔵FM音源とMIDI音源の切り替えをすることはできません。

その理由として考えられるのは、MIDI音源を鳴らす際でも内蔵FM音源からの音声が停止するわけではなく、音に厚みを出すためにMIDI音声が重ねて出力されているだけだからです。内蔵音源の出力とMIDI音源の出力はミキサーなどでミックスして出力しましょう。MIDIボードを装着している状態で内蔵FM音源だけの演奏にしたい場合は、MIDI音源の電源を切るかボリュームを絞ればokのはずです。(厳密に検証したわけではありません)

---

## BGMデモモード

ゲーム内テストモードとは別に、SPSが用意してくれたBGMデモ実行用バッチファイルがDiskAの中に含まれています(BGM_DEMO.BAT)。

<img src='images/bgm_demo.png'/>

これを実行するには、

1. ブランクディスクを用意し、FORMAT.Xでフォーマットする。
2. DISKCOPY.Xを使って本ソフトの DiskA をブランクディスクにコピーする。
3. コピー先ディスクの AUTOEXEC.BAT を AUTOEXEC.ORG にリネームする。
4. コピー先ディスクの BGM_DEMO.BAT を AUTOEXEC.BAT にリネームする。
5. AUTOEXEC.BATの先頭に以下を加える。

        path bgm;grp_data;b:grp_data

6. コピー先ディスクで起動する。

このBGMデモモードを実行する限りはマスターディスクを求められることはありません。

---

## デカキャラパッチ

電脳倶楽部 Vol.22 に掲載されています。本ソフトはアーケード版にくらべて処理能力を考慮してか、全体的にキャラクタが小さくなっています。これを125%まで拡大させるパッチです。XVI 16MHzで動作させた限りでは特に重くなったりといったことはありません。

---

## ハードディスクインストール

本ソフトは標準ではハードディスクインストールに対応していません。そのため2枚のFD内容をすべて読み込むため起動に120秒以上かかります。

非公式ですが、部分的にハードディスクインストールしてこの待ち時間を1/3程度にする方法です。

1. Human68k 2.02(2.03) と純正 SCSIDRV.SYS が入ったシステムディスクを用意する。(例えばGRADIUS IIのDiskAなどでokです)
2. ブランクディスクを用意し、FORMAT.Xでフォーマットする。
3. A:に Human68k v2のシステムディスク、B:にフォーマット済みディスクが入った状態で、

        A:
        SYS B:

としてブランクディスクに HUMAN.SYS を書き込む。

4. SCSIDRV.SYS をB:にコピーする。
5. SUBST.X および COMMAND.X を Human68k 3.02のシステムディスクからB:にコピーする。
6. A:を SUPER HANGON DiskA に入れ替え、HUMAN.SYS, COMMAND.X 以外のすべてのファイルを隠しファイルも含めてB:にコピーする。
7. B:のCONFIG.SYSに以下を加える

        DEVICE = \SCSIDRV.SYS /ID0

8. B:のAUTOEXEC.BATのpath文の前に以下を加える

        SUBST B: C:\SPHANGON\DISKB

9. 起動ハードディスクドライブのルートに SPHANGON のディレクトリを作り、その中にDISKBディレクトリを作る。
10. DISKBディレクトリに SUPER HANGON DiskB の内容を DISKCOPY.X を使ってすべてコピーする。

11. 以降起動する時は新しく作成したこのディスクから起動する。
12. 途中でマスターディスクを求められるので、マスターディスクと入れ替えることでゲームが起動する。DiskBは使わない。

これによりウチの環境ではカウンタが残り80を切ったくらいで起動するようになりました。
デカキャラパッチと組み合わせることも可能です。

なお、Human68k 3.02ではサウンドドライバが動作しませんので起動できません。

---

## Phantom X

Phantom X 機で本ソフトを動作させる場合は、MPU種別を68000にした上で、EMUWAITを4にする必要があります。
EMUWAIT=0~3ではOPMアクセスのタイミングが狂ってBGMが正常に鳴らなくなります。

---

## 更新履歴

2023.09.13 ... Phantom X について追記
2023.08.27 ... 初版
