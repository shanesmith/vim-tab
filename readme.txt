I am an embedded engineer working on Android platform,
and Android is such a huge system, use global and ctags encounter vary 
problems, like tag is too big, too slow to jump, global can't generate
such big tag.

I have try tmux, but find out it doesn't meet my requirement,
such as share clipboard easily, search easily.
So I written this plugin for maintain each tab different working directory,
and switch working directory automatically when switch tab,
and can also do custom actions when switch tab,like
auto reload tags in the new working directory, and some other plugin 
would also benefit from this plugin,like commandt, grep, working in a 
sub directory is so mush fast and easy.

Notes:
	1.Press 't' in NERDTree directory node will open the directory 
in a new tab, such a wonderful thing use with this plugin
	2.when first open a tab, the tab enter trigger will not be called,
the following code can be add to your .vimrc(according to your 
project file), load tags automatically when open a file
	autocmd BufEnter *.[ch] call TabReloadCGtag()
	autocmd BufEnter *.cpp call TabReloadCGtag()
	autocmd BufEnter *.java call TabReloadCGtag()

Try add the following code in your vimrc:

"reload GTAGS, ctags.
function! TabReloadCGtag()
	"reload GTAGS in current directory
	cs kill 0
	"gnu global produce GTAGS, more useful than cscope
	cs add GTAGS
	"reload tags in current directory
	set tags=tags
endfunction

"some action when enter a tab
function! TabEnterTag(nr)
	"echo "tab ". a:nr . " enter"
	call TabReloadCGtag()
endfunction

"some action when leave a tab
function! TabLeaveTag(nr)
	"echo "tab ". a:nr . " leaves"
	"nothing
endfunction

let g:TabTagTrigger = {'name':'TabTagTriger','pattern':"pattern", 'enter_callback':"TabEnterTag", 'leave_callback':"TabLeaveTag" }

"call tab#TabShowTrigger()
call tab#TabAddTrigger(g:TabTagTrigger)

"NOTE:following key map for is move between tabs
"please define the keys you like,all keys are alt+*
"input by c-v alt-*
nnoremap 1 :tabn 1<cr>
nnoremap 2 :tabn 2<cr>
nnoremap 3 :tabn 3<cr>
nnoremap 4 :tabn 4<cr>
nnoremap 5 :tabn 5<cr>
nnoremap 6 :tabn 6<cr>
nnoremap 7 :tabn 7<cr>
nnoremap 8 :tabn 8<cr>
nnoremap 9 :tabn 9<cr>
nnoremap q :call SwitchToPrevTab()<cr>
nnoremap e gT<cr>
nnoremap r gt<cr>
nnoremap c :tabclose<cr>
