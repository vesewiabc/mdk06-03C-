using System;
using System.Data.SQLite;
using System.IO;

private void ButtonBackupDb_Click(object sender, EventArgs e)
{
    try
    {
        string dir = Path.Combine(AppDomain.CurrentDomain.BaseDirectory, "backups");
        Directory.CreateDirectory(dir);
        string file = Path.Combine(dir, $"teh_db_{DateTime.Now:yyyy-MM-dd_HH-mm-ss}.db");

        using (SQLiteConnection src = new SQLiteConnection(Database.connectionString))
        using (SQLiteConnection dst = new SQLiteConnection($"Data Source={file}"))
        {
            src.Open();
            dst.Open();
            src.BackupDatabase(dst, "main", "main", -1, null, 0);
        }

        MessageBoxUI.Show($"Копия базы создана:\n{file}", "Резервная копия", "Accept");
    }
    catch (Exception ex)
    {
        MessageBoxUI.Show(ex.Message, "Ошибка резервного копирования", "Error");
    }
}
