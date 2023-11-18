# nvim-luasnip-video

```lua
-- luasnip.lua

local ls = require "luasnip"
local s = ls.snippet
local t = ls.text_node
local i = ls.insert_node
local extras = require("luasnip.extras")
local rep = extras.rep
local fmt = require("luasnip.extras.fmt").fmt
local c = ls.choice_node
local f = ls.function_node
local d = ls.dynamic_node
local sn = ls.snippet_node

vim.keymap.set({ "i", "s" }, "<A-n>", function()
    if ls.choice_active() then
        ls.change_choice(1)
    end
end)

vim.keymap.set({ "i", "s" }, "<A-k>", function()
    if ls.expand_or_jumpable() then
        ls.expand_or_jump()
    end
end, { silent = true })

vim.keymap.set({ "i", "s" }, "<A-j>", function()
    if ls.jumpable(-1) then
        ls.jump(-1)
    end
end, { silent = true })

ls.add_snippets("lua", {
    s("hello", {
        t('print("hello '),
        i(1),
        t(' world")')
    }),

    s("if", {
        t('if '),
        i(1, "true"),
        t(' then '),
        i(2),
        t(' end')
    })
})

ls.add_snippets("tex", {
    s("beg", {
        t("\\begin{"), i(1), t("}"),
        t({ "", "\t" }), i(0),
        t({ "", "\\end{" }), rep(1), t("}"),
    })
})

ls.add_snippets("cs", {
    s("logc", 
        fmt([[Debug.Log($"<color={}>{}</color>");]],
        {
            c(1, { 
                t("red"), 
                t("green"), 
                t("blue"), 
                t("cyan"), 
                t("magenta") 
            }),
            i(2),
        })),
})

ls.add_snippets("typescriptreact", {

    -- 1st version
    s("co", {
        t("position(["),
        f(function()
            local register_data = vim.fn.getreg() .. "";
            if string.match(register_data, "[%d-]+,%s*[%d-]+") then
                return register_data
            else
                print("register does not contain the pattern")
            end
        end),
        t("])"),
    })

    s("co", {
        d(function()
            local register_data = vim.fn.getreg() .. "";
            if string.match(register_data, "[%d-]+,%s*[%d-]+") then
                return M.sn(nil, {
                    M.t("position([" .. register_data .. "])"),
                })
            else
                print("register does not contain the pattern")
                return M.sn(nil, { })
            end
        end),
        i(1)
    })
})`

```
