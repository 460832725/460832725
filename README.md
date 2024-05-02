@@ -3,7 +3,8 @@ name： 推送格式
在：
  推送：
    分行：
      - 主要
      - 主人
      - 开发

权限：
  内容：写
  拉取请求：写入
职位：
  push_format：
    运行： ${{ matrix.os }}
    策略：
      矩阵：
        python版本：[“3.10”]
        操作系统： [ubuntu-latest]
      快速失效：false
    步骤：
      - 用途：动作/checkout@v3
        与：
          参考： ${{github.ref_name}}
      - name： 设置 Python ${{ matrix.python-version }}
        用途：Actions/Setup-python@v4
        与：
          python版本：${{ matrix.python-version }}
      - 名称：安装黑色
        运行：pip install “black[jupyter]”
      - 名称： Run Black
        # 运行： 黑色 $（git ls-files '*.py'）
        运行：黑色。
      - 名称：提交回
        出错时继续：True
        id：提交
        运行： |
git config --local user.email “github-actions[bot]@users.noreply.github.com”
git config --local user.name “github-actions[bot]”
git add --all
git commit -m “格式化代码”
      - name：创建拉取请求
        if： steps.commitback.outcome == '成功'
        出错时继续：True
        用途：Peter-Evans/create-pull-request@v5
        与：
          delete-branch：真
          body：应用代码格式化程序更改
          title： 应用代码格式化程序更改
          commit-message：自动代码格式
