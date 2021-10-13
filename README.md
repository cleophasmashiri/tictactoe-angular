# Tictactoe

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 12.2.3.

## Interesting part
<p>
  The function strategicPlay() enables the computer to compete with the human player.
</p>

---

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
  title = 'Tic-Tac-Toe';
  players = ['X', 'O'];
  currentPlayer = this.players[0];
  winner = '';
  mat: any[] = [];
  constructor() {
  
  }
  ngOnInit(): void {
    this.drawGame();
  }

  drawGame(): void {
    this.winner = '';
    this.mat = new Array(3).fill([]).map(() => new Array(3).fill(''));
  }

  handleStart(): void {
    this.drawGame();
  }

  handleClick(row: any, col: any): void {
    this.play(row, col);
  }

  play(row: any, col: any): void {
    if (this.winner !== '' || this.mat[row][col] !== '') return;
    if (this.currentPlayer === this.players[0]) {
      this.mat[row][col] = this.players[0];
    } else {
      this.mat[row][col] = this.players[1];
    }
    if (this.isGameOver(row, col))
      this.winner = this.currentPlayer;
    else {
      this.currentPlayer = (this.currentPlayer === this.players[0]) ? this.players[1] : this.players[0];
      if (this.currentPlayer === this.players[1]) {
        this.strategicPlay();
      }
    }
  }

  isGameOver(row: any, col: any): boolean {
    let curr = this.mat[row][col];
    let isOver = true;
    for (let r = 0; r < this.mat.length; r++) {
      if (this.mat[r][col] !== curr) isOver = false
    }
    if (isOver === true) return true;
    isOver = true;
    for (let c = 0; c < this.mat.length; c++) {
      if (this.mat[row][c] !== curr) isOver = false
    }
    if (isOver === true) return true;
    // diagonals
    if (this.mat[0][0] === curr && this.mat[1][1] === curr && this.mat[2][2] === curr) return true;
    if (this.mat[2][0] === curr && this.mat[1][1] === curr && this.mat[0][2] === curr) return true;
    return false;
  }

  openSites(): any[] {
    let sites: any[] = [];
    for (let r = 0; r < this.mat.length; r++) {
      for (let c = 0; c < this.mat.length; c++) {
        if (this.mat[r][c] === '')
          sites.push([r, c]);
      }
    }
    return sites;
  }

  getWinnableSites(): any[] {
    let defensiveCandidate: any[] = [];

    for (let r = 0; r < this.mat.length; r++) {
      let possibleCol = -1;
      let colWiseCheck = '';

      for (let c = 0; c < this.mat.length; c++) { // col wise check
        colWiseCheck += this.mat[r][c];
        if (this.mat[r][c] === '') possibleCol = c
      }
      if (colWiseCheck == 'OO') return [r, possibleCol];
      if (colWiseCheck == 'XX') defensiveCandidate = [r, possibleCol];

      // row wise check
      let c = r;
      let possibleRow = -1;
      let rowWiseCheck = '';
      for (let r = 0; r < this.mat.length; r++) { // col wise check
        rowWiseCheck += this.mat[r][c];
        if (this.mat[r][c] === '') possibleRow = r
      }
      if (rowWiseCheck == 'OO') return [possibleRow, c];
      if (rowWiseCheck == 'XX') defensiveCandidate = [possibleRow, c];
    }
    // diagonal check
    const diagCheck1 = this.mat[0][0] + this.mat[1][1] + this.mat[2][2];
    if (diagCheck1 === 'OO') {
      if (this.mat[0][0] === '') return [0, 0];
      if (this.mat[1][1] === '') return [1, 1];
      if (this.mat[2][2] === '') return [0, 2];
    } else if (diagCheck1 === 'XX') {
      if (this.mat[0][0] === '') defensiveCandidate = [0, 0];
      if (this.mat[1][1] === '') defensiveCandidate = [1, 1];
      if (this.mat[2][2] === '') defensiveCandidate = [2, 2];
    }

    const diagCheck2 = this.mat[2][0] + this.mat[1][1] + this.mat[0][2];
    if (diagCheck2 === 'OO') {
      if (this.mat[2][0] === '') return [2, 0];
      if (this.mat[1][1] === '') return [1, 1];
      if (this.mat[0][2] === '') return [0, 2];
    } else if (diagCheck1 === 'XX') {
      if (this.mat[2][0] === '') defensiveCandidate = [2, 0];
      if (this.mat[1][1] === '') defensiveCandidate = [1, 1];
      if (this.mat[0][2] === '') defensiveCandidate = [0, 2];
    }

    return defensiveCandidate;
  }

  strategicPlay(): void {
    const strategicSites = this.getWinnableSites();
    if (strategicSites.length > 0) { // strategic play
      const [row, col] = strategicSites;
      this.play(row, col);
    } else { // random play
      const sitesAval = this.openSites();
      if (sitesAval.length === 0) return;
      const randomIndex = Math.floor(Math.random() * (sitesAval.length - 1));
      const [row, col] = sitesAval[randomIndex];
      this.play(row, col);
    }
  }
}
```
<br>

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.
