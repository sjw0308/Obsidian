시작
```
\documentclass[11pt]{article}
\usepackage[review]{acl} % Required for inserting images
\usepackage{times}
\usepackage{latexsym}
\usepackage[T1]{fontenc}
\usepackage[utf8]{inputenc}
\usepackage{graphicx}
```
- 위에있는거 붙여넣고 걍 시작
- acl.sty파일, acl_natbib.bst파일 도 넣어주기
- ```
	\usepackage{microtype}
	\usepackage{inconsolata}
  ```
- 위 두개는 각각 문자 간격 조정, 고정폭 사용 등에 필요한데 걍 붙이면 될듯?

Title
```
\title{제목}
\author{이름}
\date{날짜}
\begin{document}
\end{document}
```
- 날짜는 필요없으면 없애도 됨
- 나머지 내용 전부 begin document랑 end document안에 넣음

Section
```
\begin{abstract} 아니면 \section{섹션 이름}
```
- 위처럼 넣고 abstract는 end로 닫아줘야됨. section들은 필요 없음
- 이때 각 section들을 폴더에 파일을 따로 모으고 각 파일에 section을 작성후 ```\input{파일}``` 로 불러오면 편하게 관리할 수 있음

부가적인 사항들
- Footnote
	- 숫자 작게해서 추가내용 넣는 그거
	- ```\footnote{내용}```
- ```~\ref{asdfasdf}```로 asdfasdf에 해당하는 이름을 역으로 넣을 수 있음. LaTex에서 Table이나 Figure이름이 자동으로 붙어서 역으로 ref하는 거임
- Figure
	- 아래는 예시
	- ```
\begin{figure}[t]
  \includegraphics[width=\columnwidth]{example-image-golden}
  \caption{A figure with a caption that runs for more than one line.
    Example image is usually available through the \texttt{mwe} package
    without even mentioning it in the preamble.}
  \label{fig:experiments}
\end{figure}
	  ```
	  - 이건 결과
	  - ![[Pasted image 20260916171731.png]]
	  - 아래는 다른 예시
	  - ```
\begin{figure*}[t]
  \includegraphics[width=0.48\linewidth]{example-image-a} \hfill
  \includegraphics[width=0.48\linewidth]{example-image-b}
  \caption {A minimal working example to demonstrate how to place
    two images side-by-side.}
\end{figure*}
	    ```
	- 아래는 결과
	- ![[Pasted image 20260916171921.png]]
- Table
	- 아래는 예시
	- ```
\begin{table}
  \centering
  \begin{tabular}{lc}
    \hline
    \textbf{Command} & \textbf{Output} \\
    \hline
    \verb|{\"a}|     & {\"a}           \\
    \verb|{\^e}|     & {\^e}           \\
    \verb|{\`i}|     & {\`i}           \\
    \verb|{\.I}|     & {\.I}           \\
    \verb|{\o}|      & {\o}            \\
    \verb|{\'u}|     & {\'u}           \\
    \verb|{\aa}|     & {\aa}           \\\hline
  \end{tabular}
  \begin{tabular}{lc}
    \hline
    \textbf{Command} & \textbf{Output} \\
    \hline
    \verb|{\c c}|    & {\c c}          \\
    \verb|{\u g}|    & {\u g}          \\
    \verb|{\l}|      & {\l}            \\
    \verb|{\~n}|     & {\~n}           \\
    \verb|{\H o}|    & {\H o}          \\
    \verb|{\v r}|    & {\v r}          \\
    \verb|{\ss}|     & {\ss}           \\
    \hline
  \end{tabular}
  \caption{Example commands for accented characters, to be used in, \emph{e.g.}, Bib\TeX{} entries.}
  \label{tab:accents}
\end{table}
	  ```
	- 이건 결과
	- ![[Pasted image 20260916171752.png]]
	- 아래는 다른 예시
	- ```
\begin{table*}
  \centering
  \begin{tabular}{lll}
    \hline
    \textbf{Output}           & \textbf{natbib command} & \textbf{ACL only command} \\
    \hline
    \citep{Gusfield:97}       & \verb|\citep|           &                           \\
    \citealp{Gusfield:97}     & \verb|\citealp|         &                           \\
    \citet{Gusfield:97}       & \verb|\citet|           &                           \\
    \citeyearpar{Gusfield:97} & \verb|\citeyearpar|     &                           \\
    \citeposs{Gusfield:97}    &                         & \verb|\citeposs|          \\
    \hline
  \end{tabular}
  \caption{\label{citation-guide}
    Citation commands supported by the style file.
    The style is based on the natbib package and supports all natbib citation commands.
    It also supports commands defined in previous ACL style files for compatibility.
  }
\end{table*}
	  ```
	- 이건 결과
	- ![[Pasted image 20260916172045.png]]
- Hyperlink
	- ```\pdfendlink, \pdfstartlink```
	- 위 명령어로 pdf의 마지막과 처음으로 가는 hyperlink만들 수 있음(안쓸듯?)
- Citation(Reference로 이동하는 거)
	- 일단 