# v1.1.1 文件完整性核对（行为测试之前）

核对时间：2026-11-18（英国时间），由程序逐字比较，不依赖模型判断。

1. cases.md 的案例部分与 v1.1.0 SKILL.md 的 "## Cases (practice on these)" 及其后全部内容逐字相同：是
2. v1.1.1 SKILL.md 在 "## Cases" 之前的规则正文，与 v1.1.0 相比只有一处改动——"(see Case 1 and Case 4)" 改为 "(see Case 1 and Case 4 in `cases.md`)"：是
3. cases.md 中的案例标题（顺序）：
   - ### Case 1: One tool named for one task
   - ### Case 2: Things to learn, like photography, cooking, gardening
   - ### Case 3: Things I can do now, like writing a plan or making a budget
   - ### Case 4: A French cooking channel
   - ### Case 5: A long message with examples buried inside
   - ### Case 6: Real plans, not illustrations
4. SKILL.md 和 cases.md 位于同一文件夹 examples-are-not-requirements/，发布包 ZIP 同时包含两个文件。
5. SKILL.md 末尾的案例索引（6 个名称）与 cases.md 中的 6 个案例一一对应；正文中引用的 Case 1、Case 4 在 cases.md 中存在。
