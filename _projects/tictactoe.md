---
title: Tic-Tac-Toe Friends
order: 2
---
Play Tic-Tac-Toe in real-time using ASP.NET SignalR. The project is hosted on [TicTacToe GitHub](https://github.com/splttingatms/TicTacToe) and a [live demo](http://tictactoefriends.azurewebsites.net/) is available. The game is using [SignalR](http://www.asp.net/signalr) to establish and maintain bi-directional connections between the server and player. This allows the game to push updates to players when game states change!

The great thing about using SignalR is the automatic technology fallback. The preferred technology is Web Sockets, but if the client does not support sockets, SignalR will automatically fallback to other technologies. 

![TicTacToe](/assets/profile_tictactoe.png){: .img-responsive }