Hello, my name is Steven Blowers, I am a software development engineer at findmypast.

This talk is called Creating a Microsoft SQL Server Adapter for Ecto 2.0. It will explore the technology choices made to deliver fast and the developer experience of creating an Ecto adapter.

Let's start off with a short bit of background on why it was needed. We were breaking off part of our C#/.NET monolith into an Elixir microservice, which involved taking a chunk of the database with it as well.

We wanted to benefit from the latest version of Ecto and Postgrex, and as you can't run multiple versions of Ecto on the same app (which would be crazy anyway) we needed a SQL server adapter that was Ecto 2.0 compatible.

There already existed an adapter for SQL server called TDS, however it had been stale for over a year and having browsed the repo we were struggling to get our heads around the code base, so we decided to start afresh and do our own thing.

Going into the project we set ourselves a few principles that would guide decisions we made as we were completing the project. The main ones were that we wanted to deliver fast, before and after completing this project. This meant that we would not aim to fully support all the features of SQL server and that we would write as little code as possible.

Writing as little code as possible meant composing already existing tech in order to avoid writing it ourselves. We had to find tech that would narrow the gap between SQL server and Elixir/Ecto.

Currently Microsoft is making moves to make their software compatible on Linux and mac OS. One of the ways they have done this is with the creation of the Microsoft ODBC Driver for SQL Server on Linux and mac OS.

We used this in order to benefit from first-party support and to avoid having to write a driver ourselves.

With that in place we then looked for a pre-existing library that we can use to talk to an ODBC driver, not finding one in Elixir, we broadened our search to the full BEAM ecosystem and found Erlang ODBC.

Using Erlang ODBC has been quite the double-edged sword in this project. Although it has saved us a lot of effort in talking to an ODBC driver and supports everything we needed from it at the time, it has become the most frequent limiting factor in being able to support issues requested by users of these drivers. Mostly supporting more recent SQL server column types.
