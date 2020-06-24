<div class="board">
  {#if winner}
    <h3>{winner}</h3>
  {:else}
    <h3>{status}</h3>
  {/if}

  <div class="board">
    {#each squares as square, i}
      <Square value={square} onClick={() => onClick(i)} />
    {/each}
  </div>

  {#if winner}
    <button on:click={restartGame}>Restart Game</button>
  {/if}
</div>

<script>
  import Square from "./Square.svelte"
  let winner = null;
  let squares = [null, null, null, null, null, null, null, null, null]
  let xIsNext = true
  $: status = "Next Player: " + (xIsNext ? "X" : "O")

  function onClick(i) {
    if (!squares[i] && !winner) {
      squares[i] = xIsNext ? "X" : "O"
      xIsNext = !xIsNext
      winner = assessWinner(squares)
    }
  }

  function assessWinner(squares) {
    const winningCombo = [
      [0, 1, 2],
      [3, 4, 5],
      [6, 7, 8],
      [0, 3, 6],
      [1, 4, 7],
      [2, 5, 8],
      [0, 4, 8],
      [2, 4, 6]
    ]
    for (let i = 0; i < winningCombo.length; i++) {
      const [a, b, c] = winningCombo[i]
      if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c])
        return `Winner: ${squares[a]}`
    }
    const isDraw = squares.every(square => square !== null);
    return isDraw ? "It's a draw" : null;


  }

  function restartGame() {
    squares = [null, null, null, null, null, null, null, null, null];
    xIsNext = true;
    winner = null;
  }
</script>


<style>
  h3 {
    color: red;
  }

  .board {
    display: flex;
    flex-wrap: wrap;
    width: 300px;
  }
</style>