<script lang="ts">
	import ButtonComponent from '$lib/ButtonComponent.svelte';
	import TerminalComponent from '$lib/TerminalComponent.svelte';
	import BingoIconComponent from '$lib/BingoIconComponent.svelte';
	import katex from 'katex';
</script>

<div class="flex w-full place-content-center">
	<div class="flex w-full flex-col place-items-center gap-8 lg:w-[80%]">
		<ButtonComponent class="mb-8">
			<h3 class="p-2">Bingo Problem</h3>
		</ButtonComponent>
		<TerminalComponent title="the bingo problem?">
			<p>
				The bingo problem is a math problem that I have been trying to solve for quite a while now,
			</p>
			<p>over a year as of writing this</p>
			<p></p>
			<p>
				You have an {@html katex.renderToString('n \\times n')} bingo board, with a "free" space in the
				middle. ({@html katex.renderToString('n')} must be odd).
			</p>
			<p>The board begins with all spaces filled in.</p>
			<p>
				How many unique boards can be created by un-filling {@html katex.renderToString('n')} cells that
				result in 0 bingos?
			</p>
			<p>You cannot un-fill the "free" space</p>
			<p></p>
			<p>
				The goal is to find a "closed form" (a.k.a. pure math) solution that returns the answer for
				any {@html katex.renderToString('n')} value
			</p>
		</TerminalComponent>

		<!-- Example -->
		<div
			class="bg-tarblue-900/30 border-tarblue-700 grid w-full grid-cols-1 gap-4 rounded-xl border-2 p-4 lg:grid-cols-[1fr_auto] lg:rounded-none lg:border-0 lg:bg-transparent lg:p-0"
		>
			<TerminalComponent title="an example">
				<p>Here's one example valid bingo board for {@html katex.renderToString('n=5')}</p>
				<p>(the blue ones are "filled in" and the red ones aren't)</p>
				<p></p>
				<p>you can see that both diagonals have a cell un-filled, so no diagonal bingos</p>
				<p>and each column has one cell un-filled, so no vertical bingos</p>
				<p>and each row has one cell un-filled, sp no horizontal bingos</p>
			</TerminalComponent>
			<ButtonComponent class="mx-auto lg:mx-0">
				<div class="bg-tarblue-800 h-48 w-48 rounded-xl">
					<div class="m-4">
						<BingoIconComponent
							interactive={false}
							boardState={[
								false,
								true,
								true,
								true,
								true,
								true,
								true,
								true,
								true,
								false,
								true,
								true,
								true,
								false,
								true,
								true,
								false,
								true,
								true,
								true,
								true,
								true,
								false,
								true,
								true
							]}
						></BingoIconComponent>
					</div>
				</div>
			</ButtonComponent>
		</div>

		<!-- Yapping -->
		<TerminalComponent title="deceptively difficult">
			<p>This problem is very similar to the 8-queens problem</p>
			<p>in which you have an 8 by 8 chess board and 8 queens</p>
			<p>
				then you find how many ways you can place the queens so that no queen is in the path of
				another
			</p>
			<p></p>
			<p>
				The issue with the 8-queens problem is that it has no closed form solution for any {@html katex.renderToString(
					'n \\times n'
				)} board with {@html katex.renderToString('n')} queens (as far as i could find)
			</p>
			<p>and a LOT of people smarter than me have tried that one</p>
			<p></p>
			<p>
				The bingo problem has a lot of complexity the deeper you look into it, and it makes me fear
				that i may never find a closed form solution
			</p>
		</TerminalComponent>
		<TerminalComponent title="the first way i tried">
			<p>
				The first method I tried still feels the closest to working out of any of my methods, but is
				still not quite there
			</p>
			<p></p>
			<p>
				This method thinks about the board as a collection of individual columns, where only one
				cell is un-filled in each column
			</p>
			<p>
				doing it this way automatically gets us part of the way there, with us only ever un-filling
				the correct number of cells ({@html katex.renderToString('n')} cells) in a given board
			</p>
			<p></p>
			<p>I identified 4 simple criteria a board needs to meet in order to be considered "valid"</p>
			<p>1. A cell is unfilled in the top-left to bottom-right diagonal</p>
			<p>2. A cell is unfilled in the bottom-left to top-right diagonal</p>
			<p>
				3. A cell in some column other than the "free" space column has a cell un-filled in the
				"free" space row
			</p>
			<p>
				4. The remaining colums each have a cell removed from a "unique" row (meaning a row without
				any cells already un-filled)
			</p>
		</TerminalComponent>

		<!-- Going Through the Motions -->
		<TerminalComponent title="generating boards">
			<p>Let's find all of the unique ways of fulfilling the first 3 criteria</p>
			<p></p>
			<p>
				Since no single un-filled cell cal meet multiple of the first 3 criteria, 3 columns must be
				used
			</p>
			<p>
				No cell can be un-filled in the column of the "free" cell that meets any of the 3 criteria,
				so we have to pick 3 of the other columns
			</p>
			<p></p>
			<p>That leaves us with columns that can be meet any of the 3 criteria.</p>
			<p>
				The number of permutations of picking 3 of those columns is: {@html katex.renderToString(
					'(n-1)(n-2)(n-3)'
				)}
			</p>
			<p class="font-bold">BUT THERE'S A PROBLEM...</p>
		</TerminalComponent>

		<!-- Issue #1 -->
		<div
			class="bg-tarblue-900/30 border-tarblue-700 grid w-full grid-cols-1 gap-4 rounded-xl border-2 p-4 lg:grid-cols-[1fr_auto] lg:rounded-none lg:border-0 lg:bg-transparent lg:p-0"
		>
			<TerminalComponent title="dang it...">
				<p>Some of the permutations our method generates are invalid</p>
				<p></p>
				<p>If criteria 1 and 2 are fulfilled on opposite columns,</p>
				<p>then 2 cells are unfilled in the same row</p>
				<p>
					meaning that it's impossible to get zero bingos (since only {@html katex.renderToString(
						'n'
					)} cells can be un-filled)
				</p>
				<p></p>
				<p>
					to correct for this we just need to make sure that criteria 1 and 2 aren't chosen to be on
					opposite columns (mirrored positions across the vertical center line)
				</p>
				<p>
					the corrected expression for the number of permutations of the 3 criteria is: {@html katex.renderToString(
						'(n-1)(n-3)(n-3)'
					)}
				</p>
			</TerminalComponent>
			<ButtonComponent class="mx-auto lg:mx-0">
				<div class="flex flex-col gap-2">
					<div class="flex flex-col items-center gap-2 sm:flex-row">
						<span class="place-self-center sm:pr-2">this one's good</span>
						<div class="bg-tarblue-800 h-48 w-48 rounded-xl sm:h-44 sm:w-44">
							<div class="m-4">
								<BingoIconComponent
									interactive={false}
									boardState={[
										false,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										false,
										true,
										true,
										false,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true
									]}
									boardText={[
										'1',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'3',
										'',
										'',
										'2',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										''
									]}
								></BingoIconComponent>
							</div>
						</div>
					</div>
					<div class="flex w-full flex-col items-center gap-2 sm:flex-row sm:place-content-end">
						<span class="place-self-center sm:pr-2">this one's not</span>
						<div
							class="bg-tarblue-800 h-48 w-48 self-center rounded-xl sm:h-44 sm:w-44 sm:self-end"
						>
							<div class="m-4">
								<BingoIconComponent
									interactive={false}
									boardState={[
										false,
										true,
										true,
										true,
										false,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										false,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true,
										true
									]}
									boardText={[
										'1',
										'',
										'',
										'',
										'2',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'3',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										'',
										''
									]}
								></BingoIconComponent>
							</div>
						</div>
					</div>
				</div>
			</ButtonComponent>
		</div>

		<!-- Criteria 4 -->
		<TerminalComponent title="remember criteria 4?">
			<p>Next we'll work on criteria 4.</p>
			<p>
				We have {@html katex.renderToString('n-3')} columns left to un-fill a cell on, and any of them
				can be unfilled on any of the {@html katex.renderToString('n-3')} "unique" rows left
			</p>
			<p></p>
			<p>
				the number of permutations for the "rest" (the {@html katex.renderToString('n-3')} remaining
				columns) would then be {@html katex.renderToString('(n-3)!')}
			</p>
			<p></p>
			<p>So, with that, the total number of bingo boards meeting our criteria should be:</p>
			<p class="py-1">{@html katex.renderToString('(n-1)(n-2)(n-3)(n-3)!')}</p>
			<p class="font-bold">BUT THERE'S A PROBLEM... again :(</p>
		</TerminalComponent>

		<!-- OBJECTION (a.k.a Issue #2) -->
		<div
			class="bg-tarblue-900/30 border-tarblue-700 grid w-full grid-cols-1 gap-4 rounded-xl border-2 p-4 lg:grid-cols-[1fr_auto] lg:rounded-none lg:border-0 lg:bg-transparent lg:p-0"
		>
			<TerminalComponent title="objection!">
				<p>
					Our current method doesn't count any invalid boards anymore, but it does count some
					duplicate boards
				</p>
				<p></p>
				<p>
					The board shown on the right can be created from 2 different initial first 3 criteria
					placements.
				</p>
				<p>
					(in order of columns from left to right) it could be criteria 1, criteria 2, blank, blank,
					criteria 3
				</p>
				<p>or it could be criteria 1, blank, blank, criteria 2, criteria 3</p>
				<p></p>
				<p>This means that our method counts this board twice</p>
			</TerminalComponent>
			<ButtonComponent class="mx-auto lg:mx-0">
				<div class="flex flex-col gap-2">
					<div class="bg-tarblue-800 h-48 w-48 rounded-xl">
						<div class="m-4">
							<BingoIconComponent
								interactive={false}
								boardState={[
									false,
									true,
									true,
									true,
									true,
									true,
									true,
									true,
									false,
									true,
									true,
									true,
									true,
									true,
									false,
									true,
									false,
									true,
									true,
									true,
									true,
									true,
									false,
									true,
									true
								]}
								boardText={[
									'1',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'3',
									'',
									'2',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									''
								]}
							></BingoIconComponent>
						</div>
					</div>
					<div class="bg-tarblue-800 h-48 w-48 rounded-xl">
						<div class="m-4">
							<BingoIconComponent
								interactive={false}
								boardState={[
									false,
									true,
									true,
									true,
									true,
									true,
									true,
									true,
									false,
									true,
									true,
									true,
									true,
									true,
									false,
									true,
									false,
									true,
									true,
									true,
									true,
									true,
									false,
									true,
									true
								]}
								boardText={[
									'1',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'2',
									'',
									'',
									'',
									'',
									'',
									'3',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									'',
									''
								]}
							></BingoIconComponent>
						</div>
					</div>
				</div>
			</ButtonComponent>
		</div>
		<TerminalComponent title="but why?">
			<p>
				The issue stems from the board having multiple unfilled cells that meet one of the first 3
				criteria
			</p>
			<p>
				Criteria 3 doesn't cause duplicates since our method can only create boards with once
				un-filled cell meeting criteria 3
			</p>
			<p></p>
			<p>
				So the number of duplicates depends on the number of cells on a given board that meet
				criteria 1 and 2
			</p>
			<p>
				The formula for the number of times a given board appears using our method works out to:
			</p>
			<p class="py-1">{@html katex.renderToString('a\\cdot b')}</p>
			<p>
				where {@html katex.renderToString('a')} is the number of cells meeting criteria 1 and {@html katex.renderToString(
					'b'
				)} is the number of cells meeting criteria 2
			</p>
		</TerminalComponent>

		<!-- Progress -->
		<TerminalComponent title="how fix?">
			<p>so how do we account for this mathematically?</p>
			<p>thats the problem...</p>
			<p>i haven't figured that out yet</p>
			<p></p>
			<p>
				this method has, at least, allowed me to write some rather efficient programs to calculate
				the solution for a given {@html katex.renderToString('n')} value
			</p>
			<p>but a closed form solutions has evaded me thus far</p>
		</TerminalComponent>
	</div>
</div>
